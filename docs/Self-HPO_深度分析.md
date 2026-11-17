# Self-HPO（Harness-Policy Optimization）深度分析

**总表条目**：#72｜arXiv 2610.22791（v1 2026.10.16，HTML 全文）｜T2 **[高价值]**（轻评估升级→**深读坐实**）｜深读批次⑦（2026-11-17，全文）

**【一句话】**harness 与 policy **交替共进化**的可执行完整配方：**演化中的 policy 当自己的 harness 提议者**（H-step 冻结 policy 分析自身失败、每轮设计"恰好一个新机制"）+ **P-step 在选中 harness 下 GRPO 训练**（训练中从未解出的任务构成下一轮 harness 搜索集）——联合增益 **+8.5 > 个体增益之和 +6.9（超可加性精确定量）**；且**强模型知识经 harness 转移存活 OOD、经权重蒸馏不存活**（SFT 在两个 OOD 基准上跌破 Base）。

## 1 出处与身份
- 9 人机构落定：**MSR 6**（Wenlin Yao/Baolin Peng/Qianhui Wu/Hao Cheng/Jianfeng Gao + Orchard 班底）+**UVA 2**（一作 Zhepei Wei——MSR 实习生；Yu Meng）+**CMU 1**（Danqing Wang）。
- **建在 Orchard（Peng et al. 2026，2605.15040）上**：RLVR 基线=Orchard 的、BAR（Balanced Adaptive Rollout）=Orchard 组件、slime+SGLang 训练栈——MSR agentic 建模线自家基建；ThetaEvolve（Wang et al. 2025b）作者与本文班底重叠（B. Peng/H. Cheng）。
- **开源兑现**：github.com/MSR-Orchard/Self-HPO + huggingface.co/self-hpo（轻评估时未知，深读核实——升级触发器"开源"已兑现一半）。
- CC BY 4.0；Qwen3.5-35B-A3B（**35B 总参/3B active**——极小 active 量）为基座。

## 2 方法（§2）
- **形式化**：J(H,P)=期望解出率；H-step 冻结 P 提案 K=6 个 harness 候选（每个=子类化 seed agent 的 Python 类+prompt 模板，可改 step/拦截提交/换补丁/任意步介入），在搜索集 S_t 上评分选优；P-step 冻结选中 harness，GRPO+BAR 在训练集 D 上更新 policy；**S_{t+1}=P-step 中任何 rollout 都未解出的任务**（严格硬定义）；**停止=硬任务集在轮间稳定**（实际跑 2 轮：H1-P1-H2-P2）。
- **P-step 细节**：8 rollout/任务→BAR 选 4 构造成功率 0.25-0.75 的平衡组→32 组=128 轨迹/批，1 epoch，lr 1e-6，无 KL 罚，ε_low=0.2/ε_high=0.28，温度 0.95；50 更新/P-step；8×B200 单节点。
- **搜索集演化（关键设计）**：S_0=300 随机任务；之后每轮 H-step 在"上一轮 RL 从未解出"的任务上生成新失败证据——**harness 搜索跟着 policy 的演化失败模式走，而不是反复优化初始任务**（消融 −1.5 坐实）。
- 与 #71 A-RSI 同族但对象不同：A-RSI 判"燃料何时失效"刷**任务池**；Self-HPO 判"哪些任务该进 harness 搜索"刷**诊断池**——训练轨迹元信号家族在 harness 子域的落点。

## 3 实验读数
- **主表（Table 1，avg@3±std）**：Base 61.0/54.2/58.3（均 57.8）→ Self-HPO **69.5/59.7/65.2（均 64.8）**；RLVR 67.5/56.3/61.8（61.9）、Self-Harness 65.2/54.3/60.7（60.1）。OOD=SWE-Multi（9 种非 Python 语言，+5.5 over Base，Self-Harness 仅 +0.1）+SWE-Pro（+6.9）——**OOD 上 harness-only 几乎不涨、共进化照涨**。
- **超可加性（Table 3 精确核验）**：H2 配 Base policy 61.0→64.5（+3.5）；P2 配 seed harness 61.0→64.4（+3.4）；**H2+P2=69.5（+8.5）＞3.5+3.4=6.9，超 1.6 点**——轻评估摘要声明全文坐实；交叉表全单调（P1=61.7→H1+P1=67.8→H2+P1=68.6→H2+P2=69.5）。
- **SFT 蒸馏 OOD 崩塌（摘要外金子①）**：Setting 1 中 SFT（Opus 4.8 轨迹蒸馏）65.0/**49.6/49.1**——**两个 OOD 基准全跌破 Base**（54.2/58.3）；同源知识的 Meta-Harness（harness 侧转移）69.1/56.5/64.8 双 OOD 全涨。**「同一强模型的知识，经 harness 转移存活 OOD、经权重蒸馏不存活」**——转移通道决定泛化（连 #68 Opt-HCL「经 harness 蒸馏」轴的 OOD 对照版，深读前未见于摘要）。
- **消融（Table 2）**：P-step 换 SFT −4.4/RFT −1.1（RL＞RFT＞SFT）；固定搜索集 −1.5；自提议 −2.1（69.5 vs Opus 提议 71.6）——**外部强模型有用但不必需**。
- **pass@k（95% bootstrap CI）**：k=1..8 全高于 Base；pass@8 80.0→84.8（+4.8）；按难度 Medium +17.2/Easy +12.9/Hard +6.1，**Resolved 组 pass@1 −2.2（诚实披露的已解任务小幅回退）**。
- **RL 训练信号（金子②）**：最终 P-step 零成功组降至 **6.5%** vs RLVR 的 11-17%——**harness 优化顺带治了奖励稀疏**（更多组内有成功/失败对比可学）。
- **前沿对照（Table 4）**：35B-A3B+Self-HPO 69.5 ≈ Gemini 3 Pro 69.6/DeepSeek-V3.2 70.0/Kimi K2.5 70.8（1T/32B active），超 MiniMax-M2 61.0/Qwen3-Coder-480B-A35B 55.4——**3B active 摸到前沿 dense 门槛**（用户 3B-A1 梯子兴趣直接读数）。
- **搜索前沿曲线（Fig 6）**：共享 H1 至 64.3 后，harness-only 继续搜只到 65.2 平台；Self-HPO 先 P1 跳 67.8 再 H2 68.6、P2 69.5——交替与单轴的差距可视化。

## 4 设计空间分类（Table 5=引用级底册）
两轴四元组 {policy 更新：冻结/SFT/RL}×{proposer：冻结外部/冻结自身/训练独立/演化自身}×{搜索集：固定/演化}：Meta-Harness（冻结+冻结外部+固定）/Self-Harness（冻结+冻结自身+固定）/Task-CoEvolve（冻结+冻结外部+**高方差任务演化**——按跨 harness 方差选任务，vs 本文按 RL 难度）/Evo²（冻结+**训练独立 evolver**+固定）/WHALE（SFT+冻结外部+固定，非编码域）/HASE（**RL+演化自身**+固定——**最接近先例**，但把 harness 编辑放进联合 RL 动作空间、非编码域）/Self-HPO（RL+演化自身+**RL 硬任务演化**，编码域）——**唯一全演化配置**。与 #54「哪个模型该跟随」互答：不是跟随而是交替（轻评估判断坐实）；HASE=动作空间路线 vs Self-HPO=交替搜索路线的分岔点。

## 5 Proposer 工程与进化产物（金子③——准则6 可移植件）
- **Proposer 系统提示词（Fig 7 全公开）**：每轮**恰好设计一个新机制**、父 harness 方法逐字保留（增量约束）；policy/采样/步限/验证器冻结；**「永不把参考补丁或任务特定事实注入 agent 上下文——机制必须在运行时自己算信号」**（proposer 可看参考修复做诊断，机制不可夹带）——**反烘焙约束写进提示词**；实现后搜索还验证"父方法全保留+机制在真实沙箱任务上真的触发"（反死代码）。
- **四类诊断信号**（给 proposer 的失败分类学）：**coverage gap**（参考修复所需文件 vs agent 碰过的文件）/**precision gap**（该改的都改了但改错）/**submission loss**（轨迹里有真编辑但提交补丁空/畸形）/**原型不匹配**（参考修复类型 vs 实际产物）。
- **进化出的 harness=提交卫生工程四门（Fig 8/9）**：① 预算阀（临限未提交→"停止探索、现在提交"具体配方）；② 空补丁守卫（git add -N 教育）；③ **契约接地**（首次过守卫的提交扣住一次：grep 仓库测试与调用点对齐符号/签名/错误形态——「**代码库才是权威：不要发明新 API，服从已有的那个**」）；④ 补丁回退（apply 失败→提交沙箱自身 git diff）。每门至多触发一次。
- **附录 E 个案（sphinx-9658）**：同 policy 下种子 harness 接受"散文总结"提交（0 文件送评→0 分）vs 进化 harness 扣住提交报"1 个 touched file 不在补丁里"→agent 写出真 diff→通过；诚实注：该例中 PR-intent 提醒是**假阳性**。**学到的 harness 本质=把"会修"翻译成"能得分"的提交卫生层**——与 #63 corpus grounding（把隐形皮绳放长）、#79 判据暴露（这里是接地到**任务本身的权威**=仓库测试，而非基准判据）形成三角。

## 6 诚实边界
- **主基准 SWE-Verified 被明确标注 in-distribution**（训练=SWE-rebench/V2/Scale-SWE Python 任务）；SWE-rebench↔Verified 的去污染由上游论文声明、本文未自审计——**OOD 故事=语言/任务类型迁移**（SWE-Multi/Pro），不是 held-out 隔离审计（vs #62 held-out 上尾纪律）。
- Resolved 组 −2.2 pass@1（已解任务小幅回退——微型遗忘信号，连 #68）。
- 自提议 −2.1 的差距诚实给出；2 轮即停（硬任务集稳定准则）；纯编码域。
- 零成功组下降部分是提交率机制性的（门把"白干"变成"有效提交"）——非 gaming（验证器仍判对错），但增益有卫生工程成分，能力增益与提交转化增益未分离。
- avg@3±std/pass@k bootstrap CI——统计纪律良好。

## 7 库内连线
- **Q1**：共进化轴第三读数深读坐实（+7.0 OOD 保持；超可加 +8.5>6.9）——「交替而非跟随」与 #54 互答闭合；SFT-OOD 崩塌给「为什么 harness 侧转移」补了最锋利的一条证据（与 #68 Opt-HCL 同轴但带 OOD 对照）。
- **Q3**：四类诊断分类学+反烘焙提示词约束+提交卫生门=三个可移植工程件；「训练从未解出→下一轮诊断池」连 #71 时间轴家族。
- **横向结论③**：Self-HPO **打穿三引用簇**（引 #67 Liu 2026/Evo-Harness〔A-Evolve 簇〕、#66 Wu 2026/AHE Lin 2026a〔ModularRSI 簇〕、#65 Chen 2026b/Karten/Meta-Harness/RHO）=首个跨簇枢纽；GEPA=「反射式提示进化可胜过 RL」（ICLR 定式）在基线区在场。

## 8 子域底册大丰收（§4 相关工作区，全部带 arXiv ID）
**新收 ≥18 名**（待办⑮ 增补）：**WHALE** 2609.00196（H. Kim/Y. Lee/G. Lee/**C. Finn**/K. Lee——joint harness-weight 先例、冻结外部 proposer、非编码域）/ **HASE** 2607.03935（最接近对手：RL 自提议、联合动作空间）/ **Evo²** 2610.13025（"evolving the evolver"——训练独立 evolver 服务冻结 solver）/ **Task-CoEvolve** 2608.20169（按跨 harness 方差选验证任务）/ **Co-Harness** 2607.22688（Chen 2026c）/ **SIA** 2605.27276（harness+weight 双更新）/ **HELIX** 2608.13951（Fan and Huang，concurrent）/ **MetaRSI/RSI²** 2609.06396（**元-元层：给 RSI 系统的 RSI 系统**）/ **Lin 2026b** 2605.30621（Minhua Lin 系——**"会改 harness ≠ 会受益"**：拆分编辑能力与受益能力）/ **Le 2026** 2609.04518（multi-harness RL：训练增益与 harness 依赖——#54 直系）/ **HarnessBandit** 2609.13739（learnability-transferability 调度）/ **Evo-Harness** 2608.15071（context→harness 技能编译，A-Evolve 簇）/ **HarnessOpt-Bench** 2608.06301（**测"谁会优化 harness"的基准**——L5 评 proposer 能力）/ **HarnessCompass** 2608.01918（导向可泛化 harness）/ **Harness-Zero** 2609.24974（agent-as-harness 蒸馏——#68 轴）/ **RRSI** 2609.24972+**Harness²** 2610.14224+**EnvHarness** 2608.19880+SkillRL 2602.08234（**Apple 线四连**——CuiZhu/Mishra/Lee/Pfister 签名）/ **SafeEvolve** 2609.02786（安全导向共进化）；**RHO=Pan et al. 2606.05922 落定**（"Evolving agents in the dark: retrospective harness optimization via self-preference"，EMNLP 2026 Findings——#74 前作 ID 补齐）；ACE=Zhang 2026f（ICLR，#88 固定的脚手架）；**Chen 撞名四重**：HarnessForge（M. Chen 2026a）/#65 HarnessX（T. Chen 2026b）/Co-Harness（Z. Chen 2026c）/HarnessFix（Chen）——引用必须带 arXiv ID 再+1 例。**子域规模修正：≥22 → ≥40 系统**。

## 9 遗留（深读新设）
- SWE-rebench↔SWE-Verified 实例级重叠未审计（上游声明依赖）——评测分离度待第三方复核；
- 卫生工程增益 vs 能力增益未分离（控一个"只加卫生门不训 policy"的对照即可，文中无）；
- 2 轮饱和未探（第 3 轮是否仍超可加）；K=6/300 任务预算的搜索深度依赖未扫；
- 自提议 −2.1 的失分模式（提议质量 vs 提议-搜索协同）未分解——连 Lin 2026b"会改≠会受益"可直接对照；
- 与 HASE/WHALE/Evo² 无正面实验对照（仅概念表）——**交替 vs 动作空间路线无读数对决**；
- orchard 班底的自基线问题（RLVR/BAR/Orchard 同组）——非独立对照。

**裁定**：T2 **[高价值] 深读坐实**——超可加性精确定量+SFT-OOD 崩塌新发现+设计空间分类表引用级+proposer 工程件全开源；诚实边界质量高（in-distribution 标注/Resolved 回退/自提议差距全披露）。不升 T1：两轮即停、纯编码域、主基准非 held-out 审计、卫生工程成分未分离。
