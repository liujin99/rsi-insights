# Adaptive Auto-Harness（AAH）深度分析

**总表条目**：#67｜arXiv 2606.01770（v1 2026.06.01 / v3 2026.11.01，CC BY 4.0）｜T2（维持，不升 [高价值]）｜深读批次⑤（2026-11-17，全文）

**【一句话】**首个把 harness 进化放进**开放任务流**（非静态基准）的部署研究——统一框架把"知识维护"与"部署"解耦、五种前代演化器搬到同一接口对比；核心发现是 Q1 收敛侧三连证：**工件累积不保证增益、跨任务可迁移知识随分布漂移贬值、持续修订才能保住早期增益**。

## 1 出处与身份
- 作者 10 人：Zewen Liu（Emory，一作）/Zhan Shi、Yisi Sang、Bing He、Benoit Dumoulin、Hanqing Lu（Amazon）/Minhua Lin（Penn State）/Tianxin Wei（UIUC）/Dakuo Wang（Northeastern）/Wei Jin（Emory，通信）。机构=Emory+Amazon+PennState+UIUC+NEU 五机构，Amazon 占半——**产业部署视角的学术写法**。
- 代码 https://github.com/A-EVO-Lab/a-evolve 。**A-Evolve（Lin et al. 2026）= 同班人马前作**（Minhua Lin/Hanqing Lu/Zhan Shi/Bing He 重叠）——本论文是其实验室（A-EVO-Lab）自系统的任务流化重构，A-Evolve 在表内只是五个被移植基线之一（自家前作也被批：PolyBench 18.4 < static 22.2）。

## 2 问题设定：任务流三难 + regret 分解
- 三挑战命名：**D1 增长历史**（有限上下文蒸馏无界流）、**D2 任务异质**（甲知识不适乙）、**D3 分布非平稳**（旧知识贬值）。
- 形式化干净：harness=有界表示（prompt/skill/memory/tool）+容量预算 B；evolver=把历史变成 harness 的 agent 系统。
- **Prop 1 regret 分解**（固定历史与分布下的恒等式）：总 gap = **L_evolution**（可达天花板 vs 演化器类内最优任务条件构造——如只许改 prompt 的演化器改不了工具）+ **L_adaptation**（类内最优任务条件构造 vs 实际部署的任务无关 harness）。诚实注：实验只测系统对比与选择收益，**不测两个 oracle loss**；两项都无法归因到具体组件。

## 3 框架设计
- **演化工作区与可部署 harness 分离**：evolver 有独立持久工作区（任务板/研究日志〔带 pass/fail 的假设〕/架构文档/验证测试），solver 只拿可部署工件——研究态与部署态隔离（连 #64 JIT 生成器/执行器解耦，但这里是维护/部署解耦，轴不同）。
- **四阶段多 agent 演化**：Analyze（优先复发失败）→ Research（3 个并行研究员查候选解释）→ Build（翻译成 harness 变更）→ Verify（用测试检查工件）。
- **Temporal-reveal 门**：预测市场结算前延迟显式反馈——**通道级控制而非信息流保证**（答案泄漏面另查，见 §8）。
- 部署三选：current harness / **git 分支树**（task-specific 分支+共享 main，保留版本与谱系；router 用 git show 看分支内容后选择，置信阈值 0.7）/ skill 目录+检索。
- EGL（Expected-Gain-from-Learning）触发器：0.05 阈值/窗口 3——把"要不要更新"本身做成调度决策（连 #60 停滞检测，但方向相反：#60 换策略、此处省更新）。

## 4 主结果（同一 Sonnet 4.6 求解器、Opus 4.6 演化、temp 0）
- 三条录制流：**PolyBench**（5,075 个 Polymarket 任务，2026.02.06-22）/ **CTF-Dojo**（261 安全挑战，2011-2024）/ **FutureX**（503 事件预测，2026.01-04）。
- PolyBench 全任务准确率：static 22.2 → **Full System 80.9 [79.8, 82.0]**；Multi-agent 79.8 / Adaptive 77.4 / **Continual 76.9**（第二名）/ market-following 72.0（**但亏 $5,348**——跟共识无利润）/ Meta-Harness 50.8 / OctoTools 40.0 / SkillOS 21.4 / A-Evolve 18.4 / GEPA 13.4；DeepSeek/Kimi/GLM 14.0-1.4。分差：Full−Continual +4.0、Full−Market-following +8.9、Full−Multi-agent +1.1（路由边际小）。
- **deepdog/dog 价格带决胜**：所有系统 lock 带近满分，Full System 在 deepdog（入场 <0.3）得 92 分=敢于逆人群下注的决策质量。
- CTF-Dojo Pass@1：37.2 → 50.2 [44.1,56.3]；FutureX（exploratory）：31.0 → 47.3（Multi-agent 49.5 反而最高）。
- 稳定性 CV：Full 0.11 vs SkillOS 1.37（A-Evolve 0.68）——**增益与稳定性是两个独立维度**。

## 5 驱动因素与工件法证（附录 E Table 10——本篇最独特资产）
- 消融：去持久记忆=三流全线最大赤字之一→**研究态持久性是核心部件**。
- **工件指纹学**（lines 终态 prompt 行数 / net-skip 净跳过规则 / %gat 门控率 / tools 工件数 / 0-trn 直答代理）：不同演化器收敛到**完全不同的工件策略**——GEPA=格式化+零搜索（0 工具、100% 直答）、Meta-Harness=缓存事实禁搜索（"DO NOT SEARCH… VERIFIED FACTS ALWAYS OVERRIDE SEARCH RESULTS"）、Continual=不演化 prompt 但 30 个工具、Full=平衡型（97 行/13 工具/1.8% 门控）。**harness 进化的"物种分化"首次被表格化**。
- 路由天花板有限：Oracle−Naive（树内事后最优−固定 main）差距有限；**Adapt 路由在 FutureX 反而低于 Naive**——有更好的备选≠会选。
- 能力瓶颈不随演化消失：弱共识市场置信度崩、>1MB payload Pass@1 大降——三类优先级=置信校准/证据获取/大输入执行。

## 6 持续性分析（4.4——Q1 核心证据）
- **有效覆盖**（PolyBench=实际下单/CTF=非占位 flag/FutureX=调工具）：static/evolved 覆盖率 32.0/65.1（PolyBench）——**参与率与决策质量是两个轴**（Full vs Continual 条件准确率相近、全任务差主要来自参与率）。
- 覆盖增益不扩张：gained coverage 早峰、lost coverage 后期上升；**16 条运行全部指令增长，4 条整体劣于 static**；SkillOS 增益 stage2-4 逆转——终段弃权丢掉 static 答对的 248 题、换 5 题新对（弃权死亡螺旋的完整分解）；A-Evolve CTF 边际反而后期才出现（增益可以迟到）。
- **异质性≠降增益**：Gower 距离异质性上升，但与增益的关联两流反号、合并近零——D2 单独不预测。
- **漂移才是杀手**：energy distance 度量 batch 与"上次学习批次"的漂移；冻结 vs 持续演化在同一批后续任务上重放——**漂移越大冻结落后越多**（44,265 观测/5,599 任务）。→ **Takeaway 3（Q1 原文级）**："跨多样任务可迁移的知识仍会随任务分布变化失去价值；持续改进可以需要持续修订累积知识以保存早期增益"——复利的动态修正观：**不进化不只是不涨，是贬值**。

## 7 HITL（4.5）
- 85 题 FutureX、作者写消息：**Generic（"改进检索"不指源）73.8 > Specified（指定源类型）61.5 > Static 55.4**（后 65 题口径）——**更具体的指导不保证更有用**；R3 专业源档 Static 42.1→Generic 73.7（+31.6）而 R4 非英文/门控源全灭。诚实注：steered 组还带搜索凭证+演化，**增益不能单独归因指导**。

## 8 诚实边界（Limitations 全录——T1 级）
- **单条演化历史**/配置（bootstrap CI 条件于历史而非独立重跑）；长期工件老化与成本增长未测。
- **演化器计算不匹配**（ported 配置调度/资源各异，完整成本不可得）；组件效应未隔离（部分消融"intended but unverified"）。
- **信息隔离未证明**：PolyBench 全部 5,075 条轨迹索引含 outcome-matched PASS/FAIL 标签——**premature reads 及其分数影响 unresolved**；FutureX 已确认过早答案暴露+检索截止不一致（exploratory 定级）；Net$ 是零费模拟，不构成可实现交易利润。
- 离线统计复算可精确重放（输入哈希/去重计数）——可复现纪律加分。

## 9 库内连线与裁定
- **#68 撞名更正**：AAH 的 "Continual" 基线=Karten et al. 2026（Seth Karten/Chi Jin/Kiran Vodrahali 等）≠ 我库 #68 HCL（Kang et al. 2608.19013）——**同域撞名双作，引用必须带 arXiv ID**；且 AAH 未引 #68（v3 晚于 #68 v1 三个月）。新 sweep 候选：**SkillOS**（Ouyang et al. 2026，RL 训练 skill curator，AWS 系）/ **Continual Harness**（Karten，PolyBench 第二名强基线）/ A-Evolve（同实验室前作）。
- Q1：收敛侧首个任务流级实证（漂移→冻结落后的重放因果链）；"工件累积≠增益"与 SkillOS 弃权分解=**知识贬值的两个微观机制**（过时+弃权螺旋）。
- Q3：工件五指纹（lines/net-skip/%gat/tools/0-trn）加入廉价诊断族；Gower/energy 双距离=异质性与漂移的可移植度量；temporal-reveal 门=延迟反馈环境的标准件。
- 横向结论③：**harness 子域碎片化实锤**——AAH 引用簇（A-Evolve/GEPA/Meta-Harness/Karten/Ouyang）与我库深读簇（#63/#65/#60/#64）零交集，两侧互不知晓。
- **裁定**：T2 维持不升——方法学（分解定理/D1-D3 度量/法证表/诚实边界）T1 级，但读数受三无（无独立重跑、无计算匹配、无信息隔离证明）+非标基准制约；它是 Q1 收敛侧**最干净的一篇**，但不是数字最硬的一篇。

## 10 遗留
- 三流上 static Sonnet 覆盖率仅 32-85%——"always-answer static"对照缺失（论文自认 open）；漂移-贬值系数 γ 的量化值在附录 F 图 f（文本抽取丢数）；SkillOS/Karten/A-Evolve 的 arXiv ID 待补；与 #68 HCL 的三关提交守护进化读数对照未做。
