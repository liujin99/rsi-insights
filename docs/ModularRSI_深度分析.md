# ModularRSI 深度分析

**总表条目**：#66｜arXiv 2609.14857（v1 2026.09.14 / v2 10.09）｜T2 **→ [高价值]**（深读升级）｜深读批次⑥（2026-11-17，全文）

**【一句话】**harness 进化的**两级信用分配**框架——机制级（改动定位到哪个模块）+行为级（轨迹对比归因哪种执行行为），配 benchmark-disjoint 协议与 2,000 任务自建演化集；受控实验给出子域内首个"**修改空间越大反而越差**"的负结果（joint 44.19/whole 46.44 均低于 baseline 47.57 vs 模块化 52.43），且是唯一同时展示**跨演化域+跨基座模型**迁移的 harness RSI。

## 1 出处与身份
- 15 人六机构：**Beihang+Manchester+IQuest Research+M-A-P+Langboat+Hohai**（共一 Siwei Wu〔Manchester/IQuest〕/Jincheng Ren〔M-A-P/Hohai〕；Ming Zhou=Langboat；Chenghua Lin/Xianglong Liu/Bryan Dai 等）——sweep 行"Manchester+中国机构混合"落定为**学界+小型研究创业体混合**（IQuest/M-A-P/Langboat 三家）。代码 IQuestLab/ModularRSI。
- 引用簇=**第三簇**（与 AAH 簇、我库深读簇均不同）：Meta-Harness/AHE/HarnessFix/Living-Harness/HarnessForge/Self-Harness/RHO/RSEA/TACO/AutoHarness——**引 neither #65/#63/#60**；附录 A 一表对比 11 个 harness RSI 系统，是子域密度最权威的一手名单。

## 2 问题：两级信用分配
- **核心论点（引言原文级）**：可迁移 harness 自改进的关键不在"能不能改进演化经验上的表现"，而在"改动是否代表对底层执行机制的可迁移改进"；**自修改空间的扩大≠改进**，反而增加信用分配歧义——机制级（where：定位到负责组件而非大改无关部分）+行为级（what：对比聚合轨迹，区分复发 harness 缺陷 vs 任务特定推理/随机失败）。
- 对现有方法的定位：HarnessFix=只定位不归因组件；AHE=认组件但联合演化不独立归因；Meta-Harness=whole-harness 直改。

## 3 方法
- **五模块分解**（3.2）：把 Harbor 的 Terminus-2 重组为 Agent Loop（推理-动作-观察循环/恢复）/Observation Management（保留任务相关反馈、压缩噪声）/Tool Use/Context Management（保留压缩检索）/Task Completion Detection——**基建（沙箱/并行/LLM 通信）明确排除在演化域外**（演化域显式化=信用分配的前提）。
- **对照轨迹采样**（3.3）：任务按 rollout 分正/对照/负三组；对照组=同任务成败对（函数级归因）；负组=检索历史成功配对，无则单诊断；正组=质量效率机会；产出**结构化 JSON 发现**（模块+证据+理由+提案）。
- **投票式目标选择**（3.4）：相似诊断按函数合并，**票数=提供证据的不同任务数**——只修复发缺陷不修单例失败；函数级演化史防重复/防矛盾修改。
- **三验证门**（3.5）：程序检查（静态/协议）→ **Diff Review（LLM 审查改动是否含"难泛化的任务特定解/启发式"——泛化性门=准则6 在提交层的执行）** → 执行验证（批内随机 2 任务跑通）；任一门败→按记录 diff 回滚。
- **跨模块集成**（3.6）：独立演化后加一 epoch 专项去重/厘清职责/调协作；**函数库管理**（3.7）：Function Merge（库内合并）+任务感知函数选择（按自然语言描述选子集激活）——集成后**库冻结**，评测期只选不改。

## 4 演化数据与协议（4）：子域卫生学范本
- 2,000 任务**人工从 GitHub/HF/Kaggle/Linux 文档构建**，benchmark 只给类别级检索指引、零实例泄漏；多级质检：环境完整性/实用性非平凡性/**评测器有效性（测功能正确而非表面完成）**+**参照 solution.sh 须通过全部测试且 no-op 提交须得零分（反 hack 双检）**+人工复审+LLM 语义相似度筛查去基准重叠。
- 演化仅用每域 **120 任务**（TB 相关/SWE 相关各一）——小演化集即可（对照 #62 的 51,200 评测预算：质量×卫生>数量）。

## 5 结果（DeepSeek-V4-Flash-Preview 主干）
- **In-domain**：TB 2.0 47.57→52.43、SWE-V 73.40→76.40；**Pass 3 30.34→35.96（可靠性增益大于均值增益——降随机失败）**。
- **跨域迁移（Table 1 矩阵）**：TB 演化→SWE 75.80（基线 73.40）；SWE 演化→TB 49.44（基线 47.57）——增益跨演化域存活（约 in-domain 的一半）。
- **跨模型迁移（Table 2）**：DS 演化的 harness 冻结后直接换执行模型：GLM-5.2 59.55→61.80、MiniMax-2.5 41.57→44.94——**学到的是执行过程的可迁移改进而非模型特定 exploit**（子域唯一）。
- **Table 3=Q1 级受控负结果**：同轨迹同预算下 whole-harness 46.44、joint all-module 44.19 **均低于 baseline 47.57**，模块化 52.43——**"能改更多"在 benchmark-disjoint 下增加信用分配歧义而非适应能力**；中心假设受控成立。
- 单模块分解（Table 4）：五模块各自单独演化全为正（Agent Loop +0.99 最大；Observation Mgmt 步数 34.70→22.50=**效率 −35%**）；集成再 +~2——互补性实证。
- 消融（Table 5）：去 CTA 50.56/去 FLM 49.81/**去 VG 48.31（验证门最重要）**。
- 统一协议对比（Table 6）：同 harness/数据/模型/预算下 Meta-Harness 49.44、AHE 49.81 vs **ModularRSI 52.43**——差在方法不在曝光。
- 缩放（6.6）：120→240→360→600 演化任务单调上行——**高质量 benchmark-disjoint 演化数据有缩放空间**（Q1 燃料轴新数据点：燃料质量=卫生×可执行性，可缩放）。

## 6 库内连线
- **Q1 新条款**：修改空间与可迁移性负相关（受控）——"扩大自修改空间"这一 RSI 直觉在 benchmark-disjoint 下被证伪；有效的是**收窄信用分配空间**（连 #63 适应性>生成器强度、#65 类型化让编辑作用域显式——三篇同向：**约束产生可迁移性**）。
- **准则6 harness 层正面表述的三件套升维**：#49/#51 的 held-out 纪律→#66 的**演化数据也 disjoint**（不只评测分离，连演化燃料都与基准零重叠）+no-op 反 hack+diff 级泛化审查——子域卫生学新上限。
- 附录 A 的 11 系统对比表=**harness 子域密度再上调**（≥20 系统）：引用按轴归类时此表为第一手底册。
- 跨模型迁移连 #64（生成器/执行器解耦）与 #68 Opt-HCL（强模型做候选生成）——**"改进冻结在 harness、执行换便宜模型"**的又一证据。

## 7 裁定与遗留
- **裁定**：T2 **升 [高价值]**——①子域唯一跨域+跨模型双迁移读数；②"修改空间↑→歧义↑"受控负结果=Q1 级；③卫生学三件套（disjoint 数据集+no-op 质检+diff 泛化审查）=准则6 执行范本；④ 11 系统对比表。减分：增益量级 2-5%（诚实但小）、主表单主干、TB 2.0/SWE-V 两基准窄域。
- 遗留：① 附录 A 十系统（AutoHarness/Self-Harness/RHO/RSEA/TACO/HarnessForge/Living-Harness 等）**arXiv ID 与分工待查入库评估**——AHE（J. Lin，RUC 系）**≠** A-Evolve（Minhua Lin，Amazon 系）：两个 "Lin et al. 2026" 撞名，引用带 ID；② 表 3 负结果是否随模型规模变化未测（强模型信用分配能力更强？）；③ 120 任务演化集 vs 2,000 库的利用率；④ Terminus-2 基线强度依赖（换基 harness 是否复现）。
