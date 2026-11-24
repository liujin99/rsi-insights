# RSIBench-Data（数据中心 RSI 研究者 agent 的受控基准）深度分析

**总表条目**：#152｜arXiv 2607.25886（v1 2026-07-28）｜T2 **[高价值]**（引用链补扫→**用户点名深读坐实**）｜深读批次⑰（2026-11-24，全文 HTML + 附录）

**【一句话】****数据配方 RSI 线的尺子**：把「agent 当数据中心研究员」单独拎出来量——训练/服务/评测全部钉死（Tinker LoRA SFT + Harbor-E2B 沙箱），agent 只许提交训练数据+白名单训练配置，迭代改数据策略；4 个前沿 agent×6 基准的**诚实读数是负向的**：迭代在 58.33% 的设置里能超过首个有效尝试，但**过了峰值还继续搜的 78.26% 以更低分收场**——「发现-可靠性差距」命名；§5.5 同族 RSI 早期实验给了全库最值钱的负样本：**数据管线 8%→21% 明显进化、模型却始终没超过不训练的基线 33%**。

## 1 出处与身份
- **7 人，公司+学术线（坐实）**：Fanqing Meng/Lingxiao Du（共同一作）/Qiguang Chen/Ziqi Zhao/Haocheng Lu/Mengkang Hu/Michael Qizhe Shieh——**Evolvent AI + 新加坡国立大学**。**与 #151 Gödel Forest 同团队**（Meng/Du/Chen/Zhao/Lu/Hu 五人重叠）——Gödel Forest 就是在本基准上评的（方法+尺子一队，准则 6 侧注沿用）。
- **释放**：github.com/evolvent-ai/RSIBench-Data + rsibench.co；license CC BY 4.0；v1 2026-07-28 无 venue。
- **定位**：明确接在 DataComp（数据隔离）→ DataEnvGym/Curation-Bench/PostTrainBench（闭环反馈）→ Agent² RL-Bench（经验合成）的谱系上——Table 1 自评六条全 ✓（含对 Curation-Bench 只给 1 ✓ 的对比表——**rubric 自利于自家设计**，借 Curation-Bench 深读对照：其「train/eval 隔离 ✗」忽略了对手的污染审计协议，读时打折）。
- **发现路径**：#100 深读引用链（第三十六批）→ 用户点名（「尺子」优先）。

## 2 机制（§3 协议）
- **问题形式化**：固定基模 M₀+基准证据 S+预算 C；研究者策略 π 每轮出 (D_t, c_t)=π(S, H_<t)——**训练数据+白名单训练配置**（论文自认：不是 Curation-Bench 那种全冻配方，「data alone under a fully frozen training recipe」非本基准形态）；M_t=Train(M₀, D_t; c_t)，固定评测服务回「允许的选择反馈」h_t（分数/轨迹/验证器结果/诊断）；预算内自主决定继续/修改/停止，最后**盲选一个 checkpoint**（只用 H_T、不见官方分）交官方评测。
- **固定基建**：Tinker LoRA SFT 后端+固定 serving 路径+Harbor 编排 E2B 沙箱（环境/runner/验证器/计分全在服务边界内）——agent 交的是训练工件不是训练代码。
- **数据使用约束**：种子仓库/公开源可用，**评测专用任务/标签/轨迹禁入训练**（诊断自由、泄漏设防）。
- **4×6 矩阵**：agent=Claude Code Opus-4.8/Sonnet-5（high effort）+Codex gpt-5.6-sol/terra（max effort）；基准=SWE-V/Multi/Pro（各 100 题子集）+TB 2.0（全量）+GPQA Diamond（100 题）+AIME 2026（全量×4 解码）；目标模型=Qwen3.5-35B-A3B-Base；Claude Opus 4.8=固定外部 rollout 模型；**每设置 16h+ $500 Tinker**。

## 3 主结果核验（Table 2/3 + §5.2-5.4 逐行）
- **官方分（24 设置各一代表性运行）**：SWE-V：base 12.00→Opus **46.00**（10.21h/$195.90）/Sonnet 35.00/sol 33.00/terra 42.00（**2.80h/$59.79——terra 用 28% 时间拿到 91% 的分数**）；SWE-Multi：base 7.00→Sonnet **22.00**（Opus 只有 5.00=**低于 base**）；SWE-Pro：base 0.00→sol **9.00**（$300.49 最贵）；GPQA：base 61.00→sol **65.00**（2.42h/$10.37）terra 64.00（1.14h/$4.80——**最便宜的强结果**），Opus 56.00/Sonnet 52.00 均低于 base；AIME：base 30.00→sol **53.33**/Sonnet 49.17/terra 33.33；TB 2.0：base 1.12→sol **20.22**（选择分 15.73→官方 20.22 上修）。
- **资源画像**：24 设置中位 6.55h/$62.51/4.5 个有效候选（范围 1.14-14.91h/$4.80-363.77/2-15 个）；Sonnet TB 2.0 花 8.87h/$156.93 拿 5.62% vs sol 9.56h/$69.07 拿 20.22%——**官方分必须连同时间成本一起读**。
- **非单调性（本批最重要读数）**：23 个过了峰还在搜的设置里 **18 个以更低分的最终尝试收场、5 个只回到原峰**——没有一个刷出新高；「历史最优选择」能保住提交的 checkpoint，但救不了研究过程本身。
- **check↔official 差距**：12 个 GPQA/AIME/TB 设置全选了历史最优，但选择峰值与官方分平均差 **3.89 点**（Sonnet GPQA 64→52 掉 12 点；sol AIME 48.33→53.33、TB 15.73→20.22 上修）——**开发期分数不是官方分的保证**（连 #102 check↔held-out 脱钩第四标本）。
- **推理档位诊断（Sonnet-5 SWE-V，§5.4）**：max effort 把首个候选 25→36%、历史最优 44→49%、官方分 35→52%；机制=**少 1 个有效尝试但选中的数据集大得多**（149 vs 50 条训练记录、2681 vs 595 个 assistant 轮、$401.70 vs $181.70）——「effort 买的是数据构建深度，单价是搜索广度」＝深-广权衡第三标本（连 #151 K=3、Curation-Bench heavy scaffold）。

## 4 深读金子（摘要外）
- **金子① §5.5 同族 RSI 负样本（全库最值钱的诚实读数）**：Kimi-K2.6 研究者 agent 训 Kimi-K2.6 目标模型（SWE-bench Pro 100 题，20h/$2000，实用 12.3h/$738.69）：七次尝试把候选分 8%→21%（管线真在进化：修 tool-result 丢失、换 SWE-Gym 金 patch 轨迹、降学习率+调 LoRA rank、batch 命令成 1800 规范工具对），**但每个 checkpoint 都低于不训练的 33% 参照**；最终选中近 no-op 适配器（39 token/1 步/rank 1/可忽略学习率，22%）。结论自述：目标已是能干的 instruct 模型、研究者 RSI 能力不足。**「数据工厂的进步≠模型的进步」——改配方的第一诫**。
- **金子② 停止纪律=钱**：TB 2.0 各 run 在首次达到峰值后**再花 $23.79-$96.74 而无一刷新**；Sonnet AIME 前 8 个有效候选全部 <15%、第 9 个跳到 55%（换能力假设才突破——「局部修订不动时该换定性假设」机制表述）。
- **金子③ Table 3 机制四件套（过程编码，非因果消融）**：诊断真实能力缺口/验证信号内嵌（answer-agreement、环境成功轨迹、沙箱执行——**可执行/可判定的检查优于模型自评**）/监督匹配目标行为（Verified 上 final-action 数据 39% vs all-turn 8%/safe 9%；Pro 上真实提交行为 9% vs 合成 finish 动作 7%）/保留历史最优。**监督位选择第四标本**（连 #100/#102/#151：哪个行为吃 loss 是数据设计变量）。
- **金子④ 无支配者+交互效应**：三个 SWE 任务被三个不同 agent 赢；最佳-最差差 8-20 点；agent-基准交互（而非任务族标签）才是分析单位——**「换个 agent 换个排名」＝#103 方法级方差在 agent 研究者场景的复刻**。
- **金子⑤ 成本-分数解耦**：terra（GPQA 1.14h/$4.80 64%）vs Sonnet（TB 8.87h/$156.93 5.62%）——同代 agent 间效率差一个量级；论文建议官方分连同搜索成本一起报告（评测学侧贡献）。

## 5 库内连线
- **#151 Gödel Forest**：直接上下游（本表=其尺子；同队出品）——Gödel Forest 的 54.00 vs vanilla 35.00 可与本表 Sonnet-5 SWE-V 35.00 对照互证（同 agent 同基准同分）。
- **#150 Curation-Bench**：对照组——Curation-Bench=固定池内选/改（数据隔离全冻）；本表=无现成池、agent 从种子仓库**合成**数据+白名单配置（研究行为更自由）——两把尺互补，Table 1 的对比表是同队自评要打折。
- **#100 RSI-Prime**：PostTrainBench 锚互通（RSI-Prime 训出的 GLM-5.3 得 48.24 vs DataMaster〔#153〕GLM-5 骨架 31.17——训研究者 vs 造数据框架两种路线在同一尺子上的分差读数）。
- **#103 Resolution**：金子④无支配者=方法级方差读数第三来源；金子①负样本=「改进存在但 selection 认不出」的镜像（此处是「管线改进存在但训练不兑现」）。
- **#62/#91/#53**：金子②停止纪律+金子③保留历史最优=acceptor/撤走纪律家族在 agent 研究者场景的成员。
- **姊妹库 data-mixture-insights**：金子①（数据工厂≠模型）+金子②（过峰再花钱全打水漂）+金子③（监督位/验证信号）已追注接口文档。

## 6 诚实边界
**每设置单次代表性运行**（论文自认，需重复试验）；**选择与官方评测用同一任务子集**（非 held-out 泛化——论文自认「does not yet establish adaptive generalization」）；同队基准（Gödel Forest 用它评测）；公司线（Evolvent AI）；v1 无 venue；研究者身份捆绑 LLM+脚手架+effort（非单变量）；GPQA/SWE-Multi 上两个 agent 训得比 base 还差（数据策略可以有害——自身即读数）。

## 7 遗留（深读新设）
- rsibench.co 排行榜开放性与第三方提交（若开放则升格触发）。
- 重复试验方差（同设置多运行——#103 式审慎在本表的适用度）。
- TREX/RSI-Dojo/ANDES/AutoTrainess 四篇与本表的关系补挂（watch 池）。

## 8 裁定
**T2 [高价值]（深读坐实）**。加分：数据中心研究者行为的**首把受控尺**+过程可审计（结构化记录链）+金子①同族负样本的诚实披露+机制四件套+成本-分数解耦读数。不加 T1：单运行、选择/官方同子集、同队基准+公司线、无 venue、Table 1 自利对比。**升级触发器**：多运行+std；排行榜开放+第三方提交；非同队方法在其上复现（Gödel Forest 之外的独立组）。
