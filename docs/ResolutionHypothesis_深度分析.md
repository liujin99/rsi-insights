# Resolution Hypothesis（进化环的瓶颈在识别而非提议）深度分析

**总表条目**：#103｜arXiv 2611.23068（v1 2026-11-17）｜T2 **[高价值]**（轻评估⑦→**深读坐实**）｜深读批次⑱（2026-11-24，全文 HTML + 附录）

**【一句话】**「验证器瓶颈>算力>算法」判断的**实验级铁证**：进化环不缺 proposer——缺的是能分辨好坏的 selection。**10M+ LLM 调用/1,030 次独立进化 run/210 万条测试轨迹**，两层结论都坐实：① 候选级，有益工件真实存在（最强共享技能 +8.3pp），但选择是掷硬币（执行分选择 49.7%=随机；LLM judge 也不救场）；② 方法级，九篇论文 171 列的 margins 中位仅 3.4pp、**178 列无一报误差条**——比同一方法两次独立运行的差距还小，论文排名常靠运气。另附全库最锋利的一条审计：**历史大增益（10-40pp）中位 90% 可用一句话复现，修复评测后 17.6→0.8pp**。

## 1 出处与身份
- **11 人，学术主导工业参与（坐实，机构待核已修正）**：Yuanhao Zeng/Shaocheng Lan/Ao Lu/Kuang Yan/Heng Lin/Xinang Xu/Yuan Luo/Yueting Wang/Yexin Li/Ruo Shan/Kan Ren（通讯，renkan@shanghaitech.edu.cn）——**上海科技大学+字节跳动+BIGAI+南方科技大学**。Zeng/Lan 等贡献并列。
- **释放**：项目页 lineshogan.github.io/resolution-hypothesis/；license CC BY 4.0；**「接受后释放全部可复现材料与轨迹数据」——v1 阶段材料未释出**（诚实记入边界）。
- **定位**：自认「迄今最大的 agent 进化可复现性研究」。接在 GEPA/ACE/ReasoningBank/Trace2Skill/SkillOpt(-Lite)/Meta-Harness/HarnessLens/AHE 九篇方法论文之上——**不是提新方法，是给整个子域做测量学体检**。
- **发现路径**：轻评估批次⑦（2026-11-23 洪峰 50 条入库之一，T2 [高价值] 直标）→ 深读队列②头名 → 用户裁定本轮执行。

## 2 机制（§3 方差分解——方法论骨架）
- **两层方差模型**：观测分 Ŷ(A)=Y(A)+ε；Var(Ŷ)=σ²（评估噪声，within-artifact）+τ²（run-to-run 方差，between-artifacts）。**分辨率取决于问题**：识别单工件增益只受 σ² 限制；**方法间比较受 τ²+σ² 联合限制**（每次 run 抽一个新 artifact）。
- **识别需多少次 run**：N≥2((z₁₋α/₂+z₁₋β)/Δ)²(τ²+σ²)——中位 margin 3.4pp @ 80% power/α=0.05 需 **SpreadsheetBench ~5 次、OfficeQA 10 次、ALFWorld 19 次、Terminal-Bench 23 次**（TB 下四分位 margin 需 **181 次**）。
- **实验网格**：7 方法（4 text-artifact：GEPA/SkillOpt-Lite/Trace2Skill/ReasoningBank；3 harness：HarnessLens/Meta-Harness/AHE）×6 模型（GPT-5.4 nano/GPT-5.6 Luna/Terra/DeepSeek-V4-Flash/Kimi-K3/GLM-5.2）×4 基准（ALFWorld/SpreadsheetBench/OfficeQA/TB v2.1）+9 个原论文基准增益审计；**150 configurations/1,030 独立进化 runs/210 万+测试轨迹/10M+ LLM 调用/400K CPU core hours**。

## 3 主结果核验（§4-§5 逐行）
- **方法级（§4.1）**：σ=1.0-3.7pp（三个基准）/4.5pp（TB）；τ=0-12.3pp；单 run 单评估典型 std 1.7-4.1pp。**>60% 同模型同基准配置对差距 < 1 个单 run std**（逐方法 53%-79%）；单 run 表格选对四个 text-artifact 方法中真正最好的：51-73%（三基准）/**TB 仅 33%**（随机=25%）。
- **文献 margin 调查（D.3）**：九篇论文主表 178 列（171 列有优化竞争者）：**中位 margin 3.4pp（IQR 1.2-6.6）/中位 gain 9.3pp；178 列无一报误差条；只有 6 列（Trace2Skill）平均了多 run（3 seeds）；20 列明说单 run，152 列不说**。四篇论文里最强竞争者有 15 列 ≤ 不优化 baseline——竞争者分数取决于谁跑。逐论文中位 margin：GEPA 2.5/ACE 10.4/ReasoningBank 3.6/Trace2Skill 7.6/SkillOpt 3.0/SkillOpt-Lite 2.0/Meta-Harness 3.8/HarnessLens 0.8/AHE 0.0。
- **增益审计（§4.2）**：12 基准×3 模型；27 个报告增益 >3pp 的 model-benchmark 对：**一句话规则复现中位 90% 增益**；捷径类型=适应只认一种答案格式的 grader（AIME parser 只认裸整数：nano +55.3→修复后 +1.8；Luna +39.5→+2.5）/**公式单元格不重算**（SpreadsheetBench：+28.6~+38.4→修复后 +5.7~+6.7）/**「stronger result」选项恒对**（LiveMathematicianBench：+27.1~+36.8→修复后 -1.7~0.0）/近精确字符串匹配（DocVQA ANLS≥0.999：+14~+21→修复后 ≈0）。**修复后中位增益 17.6→0.8pp；修复下重进化不回增益**。SkillOpt-Lite 自报的 SpreadsheetBench 36-45pp 在本文设置下可复现（29-38pp）——大增益是真的，但来源是捷径。
- **候选级 P1（§5.1）**：提议池 n=100 技能（DeepSeek-V4-Flash 当 proposer，78 条轨迹/10 训练任务）、10 个不相交 TB 测试任务×16 rollouts=16,000 条：**Quality@K 从 49.2%（K=1）→61.8%（K=100，2h 预算）；原 timeout 下 39.7%→51.6%**；等质量零曲线对照下 K=5 起真超出、K=100 超出 +3.3~3.4pp（五个 timeout 档全部稳定——不是截断伪影）；**最强共享技能 +8.3pp（57.5 vs 池均值 49.2，最弱 41.9）**——跨任务可共享的真增益存在。
- **候选级 P2（§5.2，Table 1）**：选择池 n=100（TB+DeepSWE 轨迹来源、11 任务 5 难度级）：**执行分选择 head-to-head 49.7%（随机=50%）；回归占池 30.5% 但占被接受候选 62.1%（false accept 比随机更差的方向，区间宽不显著）；rank correlation 0.054≈0**；原生 SkillOpt-Lite/GEPA 父选择（2 rollouts）48.6%；LLM judge pointwise 45.4/pairwise 49.0/ACE 46.9——**所有选择器统计上不可区分于随机**。看满 100 个候选，没有任何 selector 达到 oracle 在 K<5 就到的质量。
- **候选级 P3（§5.3）**：8 SkillOpt-Lite+8 GEPA runs 全部中间工件重评（8 rollouts/对）：**in-loop gain 涨到 6.9-9.0pp，重评后 +0.4~-3.2pp**；τ² 从 0 涨到 4.2-24.5 平方 pp——机制=acceptance rule 每轮拿候选噪声分比当前工件分，**反复接受噪声分布上尾的幸运抽签**（引 Cawley & Talbot 2010 选择偏差+PACE false-commit），每轮噪声选择=artifact 空间上的无导向随机漫步一步。

## 4 深读金子（摘要外）
- **金子① 一句话审计法（one-rule test，方法论可搬）**：LLM agent 逐指令标记候选解释增益的指令→人工逐条核查→把最负责的一条写成单句规则钉到未进化 baseline（规则见结果后不修改）——中位复现 90%。修复=恢复官方评测或按语义评分（双 LLM judge 只看问题/参考答案/抽取答案）。**任何「大增益」报告都可先用一句话测试排除评测捷径——成本几乎为零的增益审计**。
- **金子② LMB 选项泄漏细节（出题结构可被学走）**：「stronger result」选项在 177 题中出现 85 次、**85 次全部为正确答案**；不看数学专选它=46/46 全对；60 题无此选项时 baseline 50-61 且无工件能改进。连 #51 sealed/评测泄漏家族：泄漏不必是数据泄漏，**出题结构本身就可能是泄漏**。
- **金子③ judge 不救场的机制解释**：judge 读全轨迹也不恢复识别（45.4-49.0%），原因是**judge 的读取噪声与评估噪声独立→累加而非抵消**——「多一个模型读轨迹」≠「多一次独立测量」。对 Q6（验证器瓶颈）整条线是机制级警告：**换更强的 LLM 当验证器不解决分辨率问题**。
- **金子④ 例外三例=真增益的判据**：HotpotQA 5.5-7.6pp（修复后存活，可点名机制=检索 recall +10pp）；HoVer 8-13pp（一句话只复现 43%/41%——增益分布在多条策略上，正好证明一句话测试失败≠无真增益）；SpreadsheetBench 5.7-6.7pp 存活**但来源=适应重算引擎（写值、避开不支持的函数）——诚实标注这仍是「适应评测」而非能力增益**。真增益判据=(a) 能点名机制来源 (b) 修复评测后存活 (c) 大小>分辨率。
- **金子⑤ 过程指标 13-57× 低噪声（Table 13）**：unmasked-diagnostic check rate（检查命令的失败诊断不被掩蔽的比率）评估噪声比 terminal pass/fail 低 13×-57×（八配置全表：Baseline 57×/SkillOpt-Lite 56×/Meta-Harness 57×/HarnessLens 13×）。Trace2Skill 的 post-edit verification rate +8.73pp（z=2.17）、AHE 在 Luna 上 -17.98pp（z=-4.19）——terminal 成功率全看不见的行为变化。**但定位诚实：过程指标是诊断不是选择器**——用它当选择器会面临自己的 resolution 问题。
- **金子⑥ Quality@K+等质量零曲线（隔离 proposer 的量尺）**：max-of-K 会因噪声上偏，配 empirical-Bayes matched null 校正（κ_max=100）——「proposer 好不好」从此可脱离进化环单独量。对姊妹库梯子「代理粗筛」端可直接搬。
- **金子⑦ proposer 协议范本（F.1.1 全文）**：定位「moment of wrong commitment」（锁死错误路径的决策点：未备份就改输入/信任未验证的 oracle/不做小探针就上全量/停在表面增益）；证据纪律（中间日志截断警告——长输出从中间截断，归因前先看中间；逐 run 核 verifier 输出区分失败原因；技术声明用小探针验证）；技能写作（剥离训练实例字面量保留技术深度、原则→决策点检查表、tiny verification）。**P1 成立的另一半原因=proposer 协议做到这份上**。
- **金子⑧ 程序进化结构性免疫（Limitations 内的关键边界）**：GPU kernel/ARC-AGI-2/Atari 的 program evolution 稳定累积增益——**确定性代码+便宜并行重评在结构上避开分辨率约束**。本篇结论范围=text-artifact/harness 进化（frozen weights）。
- **金子⑨ 基准分类学（E.4 四分类）**：① 不再测目标（SearchQA 闭卷即可答、分不开 DeepSeek/Luna；LMB 选项泄漏）② headroom 小（AIME 2025 DeepSeek 96.5%；ALFWorld SkillOpt-Lite 原论文 GPT-5.4-mini 以上全 100.0——**不再能排名**）③ headroom 大但无可识别增益（TB 留 28-46pp 最好方法只拿 1.0-3.6pp）④ 有真增益（三例，各能点名来源）。对 benchmark 消费者：**先问基准在哪一格，再问方法多少分**。
- **金子⑩ AI 助手使用声明（范式）**：实验/分析代码用 AI 写、作者全审负责；增益审计 LLM 标记+人工逐条核查——agent 时代论文的诚实声明范本（连 #100 人工审计 κ=0.866 的同类实践）。

## 5 库内连线
- **Q6「验证器瓶颈>算力>算法」**：本篇=该判断的实验级确认（触发器兑现）——选择 49.7%=随机、方法级 margin<运行方差、大增益=评测捷径三层证据互扣。
- **#91 PACE**：§2 直接引用其 false-commit 分析+§5.3 机制（接受噪声上尾）同族——**acceptor 纪律线的方法论上游在本篇被实验放大**；e-process 接口问题（触发器遗留）仍开放：本篇未用序贯检验，多 run 方差报告是替代方案。
- **#53 撤走纪律/#62 acceptor 家族**：in-loop 增益=噪声开采（6.9-9.0pp vs 重评 +0.4~-3.2）=「接受幸运抽签」的跨轮版本。
- **#37 验证器可读性/#51 sealed**：金子②选项泄漏+§4.2 捷径类型=评测泄漏家族新标本（泄漏在 grader/出题结构层，不在数据层）。
- **#100 RSI-Prime**：基线三运行方差 ±13 点（批次⑮记为「连 #103」）——本篇给出该方差的机制解释（τ² 随轮数累积）。
- **#102/#107 权重自改线**：9B 打印过离群统计 0.0985 而未评论=selection 瓶颈微观标本（批次⑮已记）——本篇宏观坐实；check↔held-out 脱钩标本+1。
- **#152 RSIBench-Data**：无支配者+agent-基准交互=分析单位（当时标「#103 复刻」）——**本尊入库，两层互证**：#152 量的是「过峰续搜更低收场」（非单调性），本篇量的是「为什么」（选择=随机→随机漫步）；check↔official 差 3.89 点的机制根源=σ²。
- **#150-#153 数据配方 RSI 线**：改配方第一诫（验证锚不训基线不锚上轮）的 agent 进化版=**「比上一轮好」可能是噪声开采而非真改进**——in-loop 曲线上升本身不构成证据，重评/官方评测才作数。
- **#41 reef**：meta_harness RESULTS.md 的 TB 30×2 重评打平也照记（Reef 23/60 vs 24/60）——本篇 σ=4.5pp（TB）直接解释为什么 30 题×2 的差异本来就落在噪声内：**reef 的诚实记录纪律被本篇的测量学事后正名**。
- **姊妹库 data-mixture-insights**：「proxy 排名不可信」准则的**领域级实证**（178 列无一报误差条）；N=1-2 runs 的分辨率账直接可用（见 §6 追注）。

## 6 诚实边界
**材料未释放**（v1 阶段「接受后释放」——10M 调用的数据不可独立复算，当前只能信论文表格）；v1 无 venue；候选级证据只在 TB v2.1 两个 n=100 池（局限自认）；方法学 grid 覆盖 7 方法 6 模型（自认非穷尽）；**更强模型可能噪声更低**（自认无法排除——但 TB 上 3.5-4.0pp 对三个模型都不变，ALFWorld 天花板附近缩小但 headroom 同步归零）；一句话测试只证明「一句话足够」不证明「工件无其他价值」（E.5 自认）；工业参与（ByteDance）但学术主导；程序进化不在结论范围内（结构性免疫是框架外推）。

## 7 遗留（深读新设）
- 材料释放后第三方复算（项目页盯哨——释放即触发器）。
- 与 #91 PACE 的 e-process 接口：序贯检验能否降低「多少 run 才够」的账（N 公式是固定 horizon 的）。
- 姊妹库梯子的分辨率换算：1.5B CPT 验证档的 σ/τ 实测（本篇 TB 数值不能直接搬，但测量协议可搬——8 runs×16 评估的设计成本可先用代理档试跑）。
- 程序进化例外线的库内条目化（KernelAgent/ARC-AGI-2 轴是否值得开 watch 行）。

## 8 裁定
**T2 [高价值]（深读坐实，不升 T1）**。加分：子域级测量学体检的规模之最（10M 调用/1,030 runs）+方差分解+一句话审计+Quality@K 三件可搬的方法学工具+P1/P2/P3 三预测全中+九篇论文 178 列的实证调查+真增益判据与四分类基准学+诚实边界全列。不加 T1：材料未释放（无法独立复算）、无 venue、候选级证据单基准、部分结论依赖作者自跑的审计（LLM 标记+人工核查，无第三方）。**升级触发器**：材料释放+第三方复算兑现；venue（NeurIPS/ICLR 级）落地；方法学工具（一句话审计/Quality@K）被其他组采纳。
