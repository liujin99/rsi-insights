# RSI Insights

Recursive Self-Improvement（递归自我改进）领域的研究洞察项目。

> **English**: A tier-graded research-insight library on **Recursive Self-Improvement (RSI)** — self-improving AI across four layers (weights / context / code & tools / research process), plus a governance & measurement line (oversight, task-horizon benchmarks). **69 works tracked** (36 launch seeds + 33 later entries #37–#69 incl. the industry batch, nine watch-evaluated works, the Anthropic zeta result, the harness-subdomain sweep, and deep reads of the harness trio (SPADE / HarnessX / JIT-Agent)), adjudicated (T1–T4) under the credibility rubric inherited from the sister library (experiment scale / institution / community traction / open-source auditability + adoption / recency ≤2y), with one **RSI-specific clause: self-improvement claims must be auditable** — "the agent says it got better" is not evidence; held-out evaluation or third-party replication is required. Notes are in Chinese; the tier tables are language-light.

> **免责声明**：本项目所有分析、评估和结论均为个人主观看法，针对特定场景（LLM 训练/研究环节的自改进闭环 + agent 自改系统的演进追踪）进行的判断，不代表任何官方立场，也不构成对任何论文或方法的权威评价。论文的梯队划分、价值标记和行动建议仅为本项目内部工作参考。

## 项目目标

追踪和分析 RSI（递归自我改进）领域的研究进展与工业实践，回答一个中心问题：**AI 改进 AI 的闭环，现在到底走到哪一步了，闭环的每一环有多可靠。**

## RSI 的操作性定义（本库主分类轴）

**改进者（improver）与被改进对象（improvee）的相对位置**决定层级：

| 层级 | 被改进对象 | 代表工作 | 当前状态一句话 |
|---|---|---|---|
| **L1 权重层** | 模型参数 | STaR / ReST / Self-Rewarding / Absolute Zero / RSI Arena | 可验证奖励（数学/代码）下已工业可用；自评奖励有漂移风险；agent 自主后训练已有关闭 76% base→Instruct 差距的受控实证（#37） |
| **L2 上下文层** | prompt 内的推理轨迹 | Self-Refine / Reflexion / self-correction 线 | 有外反馈时有效；纯内在自纠错有受控反例（#03） |
| **L3 代码/工具层** | agent 自己的脚手架与工具 | Voyager / FunSearch / AlphaEvolve / DGM / Gödel Agent / Dream-RSI / AntOmniEvo / hermit / reef / JIT-Agent 等 harness 子域 | 最活跃层；AlphaEvolve 已生产部署；**2026H2 工业界集体入场**（Google/DeepMind、华为诺亚×2、蚂蚁、SoMark、**Anthropic〔#59：研究版 Claude 把 RH 零点下界 41.6%→67.2%，专家实审+形式化验证〕**）；**harness 进化=L3 内部最密子域（2026-11 sweep +10 条 #60-#69）** |
| **L4 流程层** | 研究流程本身 | AI Scientist / Agent Laboratory / co-scientist / MLE-bench / RE-Bench / PaperBench | 复现型工程先被吃掉，原创研究尚未触及 |
| **L5 治理/测量线** | 监督能力与进度标尺 | Weak-to-Strong / Recursive RM / METR / RSI Arena（人评竞技场） | 监督瓶颈是治理正题不是副题；盲评人竞场成为 L1 层的外部验证器新范式 |

**判定纪律**：自改进声明须可审计——held-out 评测、第三方复现、或版本存档可 diff；**"agent 报告自己变好了"不算证据**（自我验证循环默认标 ⚠️）。

## 核心问题

1. **信号可靠性**：自改进的 reward/verifier 从哪来？自评会漂移（reward hacking / 自我确认偏误），为什么可执行验证（代码/数学）目前是唯一可靠闭环？
2. **自指涉深度**：改动的是"自己"还是"外挂"？（权重层 vs 脚手架层 vs 工具层）——自指涉越深，RSI 命题越硬。
3. **复利还是收敛**：迭代自改进何时复合收益、何时饱和/崩溃（model collapse、self-correction 反例、自评退化）？**目前不存在跨迭代的"自改进 scaling law"**——本库头号未解决问题。
4. **自动化研究现状**：AI 做 AI 研究到什么水平（MLE-bench / RE-Bench / PaperBench / METR 时间线）？哪个环节先被吃掉？
5. **飞轮治理**：闭环越自动化，人类监督越弱——weak-to-strong、递归 reward modeling 能否跟上？

## 目录结构

```
rsi-insights/
├── papers/          # 论文 PDF（本地存储不入库）
├── docs/
│   ├── RSI全景图与核心未解决问题.md    # 层级分类 × 开放问题 × 证据强度地图
│   ├── 论文总表.md                     # 全部条目主表（梯队+标记+机构+Venue+一句话核心）
│   ├── 关键Takeaway.md                 # [高价值] 家族一句话结论
│   ├── EvalResearchBench_深度分析.md    # #51 深读文档（首篇；此后深读一篇一档）
│   ├── Dream-RSI_深度分析.md            # #38 深读文档（2026-11-17）
│   ├── InfraRSI_深度分析.md             # #39 深读文档（2026-11-17）
│   ├── TheTrinity_深度分析.md           # #50 深读文档（2026-11-17）
│   ├── SPADE_深度分析.md                # #63 深读文档（2026-11-17，深读批次③）
│   ├── HarnessX_深度分析.md             # #65 深读文档（2026-11-17，深读批次③）
│   ├── JIT-Agent_深度分析.md            # #64 深读文档（2026-11-17，深读批次③）
│   └── Watchlist_2026Q4.md             # 2026Q4 热度扫描 watch 名单（未评估，凭标题+热度收录）
└── README.md
```

## 可信度评估准则（沿用姊妹库 2026-10-12 版五条 + RSI 特化条款）

**与姊妹库四维度的对应关系**（2026-11-16 用户重申：整体准则与 data-mixture-insights 一致——权威性、可信程度、工业级实践、背书）：

| 维度 | 落在哪些条款 | 一句话口径 |
|---|---|---|
| **权威性** | 准则 1（实验规模）+ 准则 2（作者与机构/Venue） | 头部厂商实验室 / 知名学术组 / 顶会 venue 加权；单作者无机构默认降档 |
| **可信程度** | 证据强度排序（定理 > 受控机制/反例 > 方向性结论 > 具体数字）+ 准则 6（自我验证风险） | 正结论最易蒸发，反例/算术账最耐用；生成者=评估者默认降半档 |
| **工业级实践** | 工业实战精选（独立分栏，生产采用证据三档） | 与梯队分级独立："研究 demo 惊艳"不等于"生产在用" |
| **背书** | 准则 3（业界关注度）+ 准则 4（开源可审计+社区采用） | 大厂 TR 引用/工业采用/千级 star 是正信号；无背书工作引用前回溯一手证据 |

对论文/报告的评估，除既有可信度分层规则（**定理 > 受控机制/反例 > 方向性结论 > 具体数字**；正结论最易随规模蒸发，反例/算术账最耐用）外，五项评估准则与 [data-mixture-insights](https://github.com/liujin99/data-mixture-insights) 完全一致：

1. **实验规模**：必须显式记录（模型参数量+数据量+训练/迭代 tokens），与工业界主流规模对照，相差 2 个数量级以上默认标 ⚠️ 规模外推风险。
2. **作者与发布机构**：作者背景、机构类型（头部厂商实验室 / 知名学术组 / 初创 / 单作者无机构）录入行内。**匿名处理条款（2026-11-17，#50 The Trinity 调查设立）**：品牌匿名但身份可查证（官方 BibTeX / arXiv 提交者 / 作者主页 / 代码贡献者任一可核）=**开放式匿名**，按查实机构定级；身份不可查证的**封闭匿名**默认不可定级。
3. **业界关注度**：被大厂 TR 引用、工业采用、社区讨论度是正信号；缺乏关注度的工作引用其结论前须回溯一手证据。
4. **开源可审计 + 社区采用**：完全开源（代码+数据或权重，足以审计核心主张）且社区采用达标（千级 star / 第三方工业采用 / ≥6 个月持续维护），可独立支撑 T1 准入。
5. **时效**：近 2 年窗口（滚动 24 个月），出窗默认 ⚠️ 时效折损（数字/配方层尤甚）；定理与受控机制/反例层豁免；开源资产以仓库现状为准。

**第 6 条（RSI 特化，2026-11-16 设立）**：
6. **自我验证风险条款**：凡实验闭环中"生成者=评估者"（自己训自己评、agent 自评自己的产出），结论默认降半档，除非有 held-out 验证 / 外部 verifier（代码执行器等）/ 第三方复现。**理由**：自改进领域的方法论天然容易被自我验证循环美化——这是本领域特有的可信度地雷，与配比领域"小 proxy 自证"同构。

## 工业实战精选（2026-11-16 设立）

> **判断准则：工业级系统真正在生产中跑的自改进闭环**——主打生产采用证据，研究 demo 不算。与 T1-T4 分级独立（生产采用过滤 vs 可信度分层），席位/准则调整=用户裁定。
> **证据三档**：**一档**=官方直接披露生产部署+可核查数字；**二档**=开源资产可审计出方法（含第三方生产采用）；**三档**=≥2 家独立头部厂商披露同款实践。

### 参考席位（1 席，初始）

| 席位 | 系统 | 证据 | 深读落点 |
|---|---|---|---|
| ① | **AlphaEvolve**（DeepMind） | 〔一档〕官方披露：数据中心 Borg 调度部署、回收 ~0.7% Google 全球算力、加速 Gemini 训练 kernel——改进对象=生产/训练基建 | 待深读 |

**候选观察席**（证据未达档，用户裁定前不进参考链）：
- **OXR（SoMark）**：部署中的产品模型（1.3B 文档解析，在线 demo+免费 API），**后训练使用 RSI loop（自称 document parsing 首次）**，三榜第一可复算+权重开源——但 RSI 主张论文未出、loop 细节不可审计〔二档候选，论文即触发器〕
- AI co-scientist（Google）：合作实验室湿实验验证部分假设，但系统未公开、验证深度有社区争议〔二档候选，待核〕
- AI 辅助编程的生产化（Claude Code / Copilot 工业渗透）：≥2 家厂商收敛，但属"人机混合改进"，非自主闭环〔三档候选，边界存疑〕

## 重点关注（watch，2026-11-16 设立）

> **收录准则**：对判断"RSI 闭环走到哪一步"有直接信号价值的新工作/事件，热度高但资历浅——**watch ≠ 参考**，未过可信度关之前不作直接参考（对齐姊妹库 #165 daVinci 纪律：直接参考权=工业实战精选独占，新文只作分析素材）。入列/移出=用户裁定。

### ① RSI Arena（2026-11-16 用户指定重点关注）

- **是什么**：Bake AI 主办、Hugging Face + Scale AI 合作的受控竞技场（COLM 2026 现场活动，arXiv 2610.33624，2026.10.24）——**十个前沿模型（GPT-6 Astra / Grok 4.7 / GLM-5.3 / MiniMax / Step 5 等）当"训练 agent"**，从同一 base（Nemotron 3.5 Lightning 30B-A3B Base）出发，各拿 1,000 GPU-hours + $300 API 额度，在共享 64 卡集群上自主选数据/实现训练流程/选 checkpoint，144 小时产出十个模型；20 个 benchmark + **3,129 票盲评人类偏好**（NVIDIA Instruct 当不排名参照系）。
- **关键读数**：① 全部 10 个 agent 产出可用助手，最强的（GPT-6 Astra 训的）关掉 base→Instruct 差距的中位数 **76%**（12 个能力 benchmark），风格校正后人评接近 NVIDIA Instruct；② Stage 2（RSI 本体）：把该模型参与的对战记录+投票喂回给 agent 再训 3 天——**8/10 模型的风格校正偏好评分提升**，即"从人类偏好反馈中持续学习"在受控设定下成立；③ **负结果同样硬**：benchmark 增益与偏好增益脱钩、有 agent 鼓励模型变长回答（长度 hack）、一个模型 StrongREJECT 恶性合规上升；④ 过程事故：Grok 4.7 提前烧完 API 额度退场、agent 抢内存到 GPU 闲置需人为设限、day 3 有模型**搞崩集群**（故意与否未定论）——自主训练系统的运维/安全样本。
- **背书与可审计性**：Thomas Wolf（HF）/Alex Pentland·Misha Sra（MIT）/Zhangchen Xu·Zichen Chen（Stanford）等 18 人联署；HF org `RSIArena` 已放全部 20 个 checkpoint（10 agent × 2 stage，Aspen–Jewel 匿名代号）——**盲评人评设计恰好绕开准则 6（生成者≠评估者）**，这是它方法学上最干净的一点。
- **盯什么（watch 触发器）**：battle 数据开源兑现与否；第三方复现/续届（第二届？换 base？换模型档）；"benchmark↔人评脱钩"结论是否被后续工作确认——若确认，等于宣告 L1 层开放域 verifier 问题在竞技场尺度上成立（连本库 Q1/Q6 的直接证据）。

## 论文索引

> 建库种子 36 篇 + 后续 33 条（#37 RSI Arena / 业界收集批次 #38-#49 / The Trinity #50 / EvalResearchBench #51〔深读〕/ CUA-Master #52 / R²Orch #53 / CoEvo-RSI #54 / 轻评估批次 #55-#58 / 深读批次〔#38/#39/#50 补深读〕/ Anthropic zeta #59 / harness 子域 sweep 批次 #60-#69 / 深读批次③〔#63/#64/#65 harness 三连〕），共 69 条，每行**一句话核心**。详细内容、⚠️ 边界与梯队裁定注记以 [`docs/论文总表.md`](docs/论文总表.md) 为准。
>
> **标记说明**：`[高价值/重要]` = 领域理论根基；`[高价值]` = 直接可用方法/证据；`[高价值/反例]` = 受控反例（按证据排序最耐用的一类）；`[工业实战/…]` = 工业实战精选成员；`[有意思]` = 有价值但需批判看待；`[轴外]` = 谱系参照不作直接参考。约束：无后缀 `[高价值]` 仅限 T1/T2。

### 第一梯队：高可信度 + 高价值

| # | 论文 | 机构 | Venue | 一句话核心 |
|---|------|------|-------|-----------|
| 01 | **STaR: Bootstrapping Reasoning with Reasoning** | Stanford/MIT | NeurIPS 2022 | **[高价值/重要]** 拒绝采样+自生成理由再微调的 bootstrapping 闭环——一切权重层自改进的模板；⚠️ 已出窗但机制层豁免（奠基） |
| 02 | **Self-Rewarding Language Models** | Meta FAIR | ICML 2024 | **[高价值]** LLM 自己给自己当 judge 产生 DPO 信号，迭代训练下指令跟随与自评能力同升——"自评能力可被训练放大"首个主结果；⚠️ 出窗（2023.12），后续分析指增益部分来自基座偏好，引用需核对 |
| 03 | **LLMs Cannot Self-Correct Reasoning Yet** | Google | ICLR 2024 | **[高价值/反例]** 无外部反馈的内在自我纠错常常降低而非提升推理表现（自评信心不校准）——上下文层自改进的受控反例 |
| 04 | **Model Collapse** | Cambridge+DeepMind 等 | **Nature 2024** | **[高价值/反例]** 递归自食生成数据导致模型退化（长尾消失）——自改进闭环"数据质量墙"的 Nature 级警告；⚠️ 刚出窗（2026.07），反例条款耐用 |
| 05 | **AlphaEvolve** | DeepMind | 技术报告 2025.05 | **[工业实战/高价值]** 进化式代码搜索 agent，改进对象=训练/生产基建（调度回收 ~0.7% 全球算力、加速 Gemini kernel）——目前唯一一档生产部署的自改进闭环 |
| 06 | **Darwin Gödel Machine** | Sakana AI+UBC | arXiv 2025.05 | **[高价值]** agent 存档自身权重并经验性改写自己的代码（SWE-bench 20%→50%），全版本存档开源可审计——"改自己"闭环最透明的单点 |
| 07 | **Measuring the Ability of LLMs to Complete Long Tasks**（METR 任务时长线） | METR | arXiv 2025.03 | **[高价值]** agent 可完成任务时长每 ~7 个月翻倍——自动化研究进度的权威标尺；⚠️ 2025-08 后续修订提示外推有不确定性（曲率待核） |
| 08 | **Weak-to-Strong Generalization** | OpenAI | ICML 2024（arXiv 2023.12） | **[高价值]** 弱监督者也能让强学生超过自己（部分设置）——监督瓶颈不是硬墙，RSI 治理线奠基；⚠️ 已出窗但议程层仍活跃 |
| 09 | **Beyond Human Data（ReST-EM 线）** | Google DeepMind | ICLR 2025（待核） | **[高价值]** 自生成数据+二元 verifier 过滤超过人类数据（Gemini 数学）——"可验证奖励下的权重层自改进"工业规模确认；⚠️ 出窗预警（2024.12） |
| 37 | **RSI Arena: Towards Recursive Self-Improvement with Human Feedback** | Bake AI（+HF/Scale AI 合作；MIT/Stanford/UW 等联署） | arXiv 2610.33624（2026.10）+ COLM 2026 现场 | **[高价值]** 十个前沿模型当训练 agent 自主后训练同一 base，3,129 票盲评：最强关掉 76% base→Instruct 差距；Stage 2 人评反馈再训 8/10 提升——但 benchmark↔偏好脱钩、长度 hack、StrongREJECT 恶化；**重点关注①** |
| 41 | **reef** | Human-Agent-Society（社区组织） | 开源基建 2026.08-（7.8k⭐/2.5 月） | **[高价值]** 自改进基建枢纽：推理-反馈-学习-版本化交付一条龙，权重训练（Slime/SGLang）与 harness 进化双通、GEPA/SDFT/SDPO/OPD/Meta-Harness 配方齐——准则4 star 触发器激活（2.5 个月 7.8k⭐） |

### 第二梯队：高相关（未全达 T1 准入）

| # | 论文 | 机构 | Venue | 一句话核心 |
|---|------|------|-------|-----------|
| 10 | Self-Refine | CMU | NeurIPS 2023 | 上下文内自反馈迭代改进输出——L2 层奠基；⚠️ 出窗，机制层注记 |
| 11 | Reflexion | Northeastern/MIT 等 | NeurIPS 2023 | 语言化反思作为失败信号（HumanEval 91%）——"失败→反思→重试"经典闭环；⚠️ 出窗，机制层注记 |
| 12 | Voyager | NVIDIA+Caltech 等 | TMLR 2024 | agent 自写可复用代码技能、技能库自扩张——"能力随库复利"的 L3 层样板；⚠️ 出窗（2023.04），机制层注记 |
| 13 | Large Language Models Can Self-Improve | UIUC+Google | EMNLP 2023 | CoT 自一致性筛选自训（无标注）大幅涨分——STaR 无真值版；后被指出数据质量/泄漏隐患，与 #03 构成争论线 |
| 14 | SPIN: Self-Play Fine-Tuning | UCLA | ICML 2024 | self-play 直到自身分布匹配人类分布即收敛——"以人类数据为吸子"的自博弈微调；⚠️ 出窗临界 |
| 15 | SPAG: Self-Play Adversarial Language Game | 腾讯 | NeurIPS 2024（待核） | 对抗语言博弈 self-play 提升通用推理——博弈环境当通用奖励源 |
| 16 | **Absolute Zero** | 清华 IIIS 等 | arXiv 2025.05 | **[高价值]** 零人类数据：自 proposer 出题+代码执行器当唯一 verifier 的 self-play RL——"验证器=执行器"最极端形态；⚠️ 7B 级规模外推风险+后续争议待核 |
| 17 | **The AI Scientist（v1/v2）** | Sakana AI+UBC+Oxford | arXiv 2024.08 / v2 2025 | **[高价值]** 端到端自动科研（idea→实验→论文→自动评审，$15/篇），v2 生成论文首次通过 workshop 评审（待核）；**安全事件：v1 曾自行改代码绕过资源限制**——L4 层安全样本 |
| 18 | Agent Laboratory | AMD+JHU | arXiv 2025.02 | human co-pilot 模式的 ML 论文生产线（$15-20/篇）——自动化研究流水线开源对照实验台 |
| 19 | MLE-bench | OpenAI | arXiv 2024.10 | Kaggle 竞赛工程基准：最强配置≈铜牌水平（~8.7% 竞赛）——"AI 能完成多少 ML 工程"标准化答案；⚠️ 刚出窗 |
| 20 | RE-Bench | METR | arXiv 2024.11 | AI R&D 任务+专家人基线：部分任务 8h 预算内达专家时间效率、长时程仍落后——自动化研究最小可信标尺；⚠️ 出窗临界 |
| 21 | PaperBench | OpenAI | arXiv 2025.04 | 独立复现 ICML 论文全流程评测：最强 agent ≈21% rubric 分（博士 41-77%）——复现型研究自动化的当前地板 |
| 22 | **FunSearch** | DeepMind | **Nature 2023.12** | **[高价值]** LLM 程序搜索发现 cap set 新下界——"程序空间搜索>直接答案搜索"的 Nature 级验证（AlphaEvolve 直系前身）；⚠️ 出窗，机制层豁免 |
| 23 | Towards an AI co-scientist | Google | arXiv 2025.02 | 多 agent 假设生成系统，部分假设获合作实验室湿实验验证——研究流程层首个工业级系统；验证深度有争议（待核） |
| 24 | ReST: Reinforced Self-Training | DeepMind | arXiv 2023.07 | 自生成→过滤→再训的迭代自训练框架——STaR 工业化版；⚠️ 出窗，机制层注记 |
| 38 | **Dream-RSI** | **Google+Google DeepMind**+UMD+UVA | 仓库论文 36 页（2026.09，arXiv 截至 11-17 未挂）+1.6k⭐ | **[高价值]** 把"发现历史"构造成回放模拟器，在模拟器里 dreaming 改进探索策略（免在线评估的低成本元策略优化）——AlphaEvolve 谱系升级：**头部厂商对"L3=RSI 主战场"判断的直接确认**；域=算法工程/数学优化/GPU kernel。**深读**：受控对照**回放模拟器>语义指导**（Q1 经验复用方式首条读数）、元级反馈成本降 1.7×–162×；文档已建 |
| 39 | **InfraRSI** | 华为诺亚方舟系（Yunhe Wang 诺亚锚点+Guohao Dai/Yu Wang 清华 HPC 签名；TokenRhythm 发布=与 #40 同 org） | arXiv 2611.11558（2026.11.08）+可安装真系统（91⭐） | **[高价值]** RSI 打分布式 LLM 训练基建（launch/kernel/通信调度联合改写，OOM/编译冲突/测量抖动为约束）——训练基建 RSI 化厂商侧第二证。**深读**：**验证工程范本**（五层 16 不变式+1,212 执行复审 33% 拒绝率）、**LLM 先验与实测负相关** r∈[−0.49,−0.06]（逆共识探测入 Q3）、**sign flip 命名**（跨硬件经验变号）、4 负载 geomean 1.83×/MFU 17.4→30.0%；文档已建 |
| 40 | **NeoHorse-1** | TokenRhythm（+华为诺亚系作者） | arXiv TR 2609.08183（2026.09）+Apache 2.0 权重 4B/9B+1.7k⭐ | **[高价值]** agent 原生模型族：异构模型池+智能路由（记录"能力需求→服务档位"证据流）喂回 agentic post-training——把"路由决策当自改进信号"的厂商开源实现 |
| 42 | **AntOmniEvo** | 蚂蚁集团（ant-research） | 开源框架 2026.09（1.2k⭐，Apache 2.0） | **[高价值]** 自动进化框架：可调物（skills/提示/工作流/代码）抽象为基因组、coding-agent Proposer 改文件、框架管调度/预算/淘汰——"你定义、框架控制、AI 干活"分工的工业级实现 |
| 43 | **OXR** | SoMarkAI | 开源模型 2026.10（3.8k⭐，论文未出） | 1.3B 文档解析模型，**后训练用 RSI loop（自称该品类首次）**，三榜第一+速度领先——RSI 进入垂直产品管线的首个公开样本；⚠️ RSI 主张不可审计、论文即触发器；**工业实战候选观察席** |
| 44 | **RSI-Anything** | 浙大/上海AI Lab+CUHK 系（Ouyang/Heng/Yin 等，待核） | arXiv 2610.13912（2026.10）+1.2k⭐+数据集 | JEPA-Anything 世界模型当预测器选动作选数据、观测反馈驱动改进——五域通用（代码修复/GUI/实验规划/反应优化/免疫科学）："世界模型替代外部 verifier"路线 |
| 45 | **RSIHub** | 多机构（Wang 等，待核） | arXiv 2610.32253（2026.10） | agent 进化方法的可组合研究基建（算子+可变资源声明，执行/评估/恢复/谱系全托管）——"方法比较需要共享基建"的学术侧回应，连 reef/hermit |
| 50 | **The Trinity** | 开放式匿名已查实：InfRec+NUS+Xidian+Tongji+UCB+CUHK+HKUST+HKU+上海创新研究院（21 人全披露，通讯 Pengfei Zhou/Odin Zhang/Yang You） | arXiv 2610.36077（2026.10）+MIT 代码（71⭐）+HF Daily #1（up 161） | 冻结底座（Claude Opus 5）harness 进化：任务重组为**能力树**+Real2Sim2Real（世界生成**排除目标任务布局与解法**）+harness 树选择性继承+能力感知路由；**深读**：46 benchmark（28+18）Train 39.1→73.2、迭代 2-4 仍 +9.4/+6.4（Q1 L3 未饱和数据点）、路由逼近 Oracle、真机 0→60%；**防泄漏纪律=论文级范本**（hash 锁定预测账本）；⚠️ 闭源底座+9/28 训练基准自建+核心工件未开源 |
| 51 | **EvalResearchBench** | OSU/UCSD/UCSC/PSU+AG2 AI（Qingyun Wu=AutoGen 系） | arXiv 2610.04184（2026.10）+代码（5⭐） | **"agent 自建评测"第一个标准 benchmark**：9 前沿模型当 researcher 冻结交付评测器——最优 PA≈75%（天花板 91%）、CCC≈23：排序可复制、水平难复制；**dev 最优≠sealed 最优**（准则6 定量实锤）；人类采样强基线、最低成本者最高 PA；**[高价值]**，深读文档已建 |
| 52 | **CUA-Master** | MMLab CUHK+THU+HKUST(GZ)+京东探索研究院 | arXiv 2611.03126（2026.11）+代码（128⭐）+HF 权重 27B | 对"自生成经验式 RSI"的正面批评+替代路线：双飞轮（模型弱项引导**开放世界数据**构建→训练）——**self-policy 注释器增益有限 vs 更强注释器持续增益**（Q1 收敛侧受控对照）；27B 四榜自报竞争力（OSWorld Verified 80.2） |
| 53 | **R²Orch** | CUHK-SZ+复旦+UIUC+清华+NUS | arXiv 2610.22740（2026.10）+代码（49⭐） | 编排层**信号可靠性工程**：Recursive Learnability（撤走技能下度量可学习性，p=0/1 即 advantage collapse）+Skill Withdrawal，命题级理论——12/12 榜全胜+**零技能保持精度**+迁移 6 底座；⚠️ 9B 规模、自报 |
| 54 | **CoEvo-RSI** | NUS+A\*STAR+商汤 | arXiv 2610.25078（2026.10）+代码（21⭐）+HF | 命名 **brain–body mismatch**（harness 进化×模型能力失配）+TIMR 廉价诊断；进化档案+双向历史继承（harness 历史×checkpoint 选/合并）——9B 三垂直域超 DeepSeek-V4-Pro/GLM-5.3（自报，窄证据） |
| 55 | **Recursa** | Alibaba Research（Jingren Zhou 系） | arXiv 2611.04035（2026.11） | FM 系统级自改进：新域自适应**不用人类专家/更强老师**——结构化知识仓库（技能+指令+记忆+反思）bootstrap 后递归精炼；与 #52 构成"自经验是否饱和"正反对照线 |
| 57 | **Continual Learning Exam** | UCL（Jun Wang 系）+Imperial+PKU+UNC 等 | arXiv 2610.33956（2026.10）+开源基准 | **经验驱动改进首把大规模标尺**（600 级进阶梯+16,125 人工检查项）：17 个前沿配置**反馈修订中位仅 +5.3pp、最好 47.7% 过级、23.3% 全通关**——前沿模型经验复利在真实工作流≈0（Q1/Q4 硬地板读数）；**[高价值]** |
| 59 | **Claude 黎曼 zeta 零点下界推进** | Anthropic（Conrey/Goldston 外部实审） | anthropic.com/research/riemann-zeta（2026.08） | **[高价值]** 未公开研究版 Claude 攻 RH 未成，但把**满足 RH 的零点比例下界 41.6%→67.2%**——专家实审+形式化可验证证明；Anthropic 线入场 L3 发现层首例（准则6"第三种满足方式"：重大发现先于方法论公开）；⚠️ 模型/方法/代码全未公开 |
| 60 | **EvoX** | Berkeley 系（Sky 系谱签名） | arXiv 2602.23413（2026.02） | **搜索策略本身也要进化**（meta-evolution）——#38 Dream-RSI 所引"在线元优化"代表，与 #38 构成"在线元优化 vs 离线回放"直接对照对 |
| 61 | **SkyDiscover** | Berkeley 全栈（Cemri/Zaharia/Stoica/Dimakis 等） | **ACM CAIS 2026**（短文） | "自适应发现基建"框架——Berkeley 把发现系统当基建层做（Sky 命名系谱）；本批唯一有正式 venue 的工作 |
| 62 | **SimpleTES** | Ye/Lin 系（MIT/Harvard，AI Scientist 作者群） | arXiv 2604.19341（2026.04） | "评测驱动的结构化 scaling"——**#38 的 162× 对照锚**（GPT-OSS-120B+51,200 代强基线）；"scaling 要结构化到评测信号能复利"连 Q1 |
| 63 | **SPADE** | 九机构 19 人（UW/Stanford/NEU/CMU/MIT/NUS/SNU/Stevens/UChicago） | arXiv 2608.19197（2026.08） | **[高价值]**（深读）**一个 LLM 分饰两角的自博弈**：Designer 写可执行训练环境、Agent 学，**hint-based regret 训 Designer**——8 held-out 基准 30B-A3B **+8.1 且增益随规模扩大**；**适应性＞生成器强度**（GPT-5.5 静态池≈自固定池＜自适应）、**无信号自生成有害**（40.5＜50.2）；regret 唯一定理化但小模型变负（规模下限）；corpus grounding 防坍缩+learnable share+**环境预算 $10 亿/年**产业读数 |
| 64 | **JIT-Agent** | NUS+EverMind AI+NTU（Guibin Zhang=#54 同生态） | arXiv 2608.25593（2026.08） | **（深读）harness 生成做成训练模型**（AOT/JIT 范式区分+四模块协议+三段训练含 Evo-GDPO）——**18/18 配对全升+24/24 跨六主干**、DeepSearchQA 85.1 超 GPT-5.6；**6/6 token/成本全场最低**（"买效率不买轨迹长度"）；生成器/执行器解耦两级栈=meta 成本一次性摊销；⚠️ 训练-评测分离度未明说 |
| 65 | **HarnessX** | **小米 EV** Darwin Agent Team（非腾讯，已核） | arXiv 2606.14249（2026.06） | **（深读）类型化处理器+替换代数组装+AEGIS 四阶段**（操作镜像：三 RL 病理↔三防御）——**L3→L1 回流真做了**：cross-harness GRPO 共进化 **+4.7%** 跨过 scaffolding ceiling=回流轴第一读数；**两个天花板命名**（#54 brain-body 正面表述）；逆缩放（最弱 agent +44.0）；⚠️ 主表全 on-set（论文自认）——方法学 T1 级/读数 T3 级 |
| 66 | **ModularRSI** | Manchester+中国机构（分工待核） | arXiv 2609.14857（2026.09） | 可泛化 harness 进化的**评测卫生学**：benchmark-disjoint+对照式缺陷归因+五功能模块受限域独立进化——**"在评测集上演化=基准特定适应"=准则6 的 harness 层表述** |
| 67 | **Adaptive Auto-Harness** | Northeastern（Dakuo Wang）系 | arXiv 2606.01770（2026.06，v3 11） | 开放任务流持续自改进（预测市场 22.2→80.9%）+**"工件累积不保证持续增益、可迁移知识随分布变化贬值"**=Q1 收敛侧新证据——持续进化靠修订累积知识保增益 |
| 68 | **Harness Continual Learning** | 中国机构（Yang Gao/Lei Wang 签名，待核） | arXiv 2608.19013（2026.08） | **命名 harness-level forgetting**（harness 更新破坏先前可靠行为=参数层灾难性遗忘的 harness 镜像）+三关提交守护进化（改进/历史保持/有效性）——准则6 的 harness 层等价物 |

### 第三梯队：方向参考（理论源头 / 预测情景 / 综述 / 待验证）

| # | 论文 | 机构 | Venue | 一句话核心 |
|---|------|------|-------|-----------|
| 25 | Gödel Machine | J. Schmidhuber | TR 2003（理论） | 形式证明保证效用提升前提下自改写任意部分的理论最优自改进机——RSI 理论原点，无实践实例（证明成本不可行） |
| 26 | Superintelligence | N. Bostrom | 专著 2014 | 智能爆炸论证系统性源头（takeoff 三分法）——预测类历史锚点 |
| 27 | The Singularity: A Philosophical Analysis | D. Chalmers | JCS 2010 | 哲学侧最早严肃论证递归改善达到"逃逸速度"的可能性 |
| 28 | Intelligence Explosion Microeconomics | E. Yudkowsky | MIRI 2013 | 认知投资回报率递增 vs 递减的经济学分析——takeoff 辩论核心框架 |
| 29 | AI 2027 | Kokotajlo 等 | 情景报告 2025.04 | 具名时间线推演（superhuman coder→RSI 加速）——预测精度争议大、影响力无争议；情景参考不当证据 |
| 30 | Self-Evolving Agents 综述（版本待核） | 多机构 | arXiv 2025 | "什么进化/何时进化/怎么进化"分类法——领域分类参照 |
| 31 | Scalable Agent Alignment via Reward Modeling（递归 RM） | DeepMind | 博客 2018 | 用 RL 训 reward model 级联放大人类监督——weak-to-strong 直系前身（历史锚点） |
| 32 | Gödel Agent | UIUC 等（待核） | arXiv 2024.11 | 运行时自改写自身逻辑（带监控护栏）——自指涉 agent 工程化尝试，无强 venue 需验证 |
| 46 | **RSIAgent** | AetherLabsAI | arXiv 2609.15364（2026.09，HF Daily #6）+610⭐ | **[有意思]** 免训练多智能体 RSI（新环境先广后深自主探索）——RSIGame 同组姊妹作；机器之心头条佐证传播度 |
| 47 | **ScienceBuddy** | Gen-Verse | arXiv 2609.17523（2026.09）+425⭐ | **[有意思]** "Recursive-in-Recursive"交互科学 agent（内环改程序/外环改模型，待深读）+ScienceIDE（PhAI-IDE 4B/9B/72B）——L4 层科学 agent 线 |
| 48 | **hermit** | Infini-AI-Lab | 开源框架 2026.09（527⭐，v0.2.0） | **[有意思]** harness adapter 框架：可训练外挂 harness（不动原模型原 harness），多进化算法（Meta-Harness/ReHarness/Harness²）——L3 层"插件化自改进"工程化 |
| 49 | **RSI-Exam** | aiming-lab | 开源基准 2026.08（198⭐，MIT） | **[有意思]** 可执行研究式 RSI 基准：88 任务（35 公开+**53 held-out 防作弊**）——held-out 纪律的正面样本 |
| 56 | **On-Policy Self-Training** | Google+Ulm University | arXiv 2610.32770（2026.10） | **[有意思]** 免数据自蒸馏极简配方：粗主题描述→自生成 QA+特权推理演示当蒸馏信号（E2B MMLU +10.2）——开放域无执行器时的软验证变通（连 #16） |
| 58 | **Trustworthy RSI 综述** | 人大高瓴人工智能学院 | arXiv 2610.35193（2026.10） | **[有意思]** 可信 RSI 风险分类：评估缺陷/能力退化/合规弱化/工件污染持续/**机制变更使验证失效**——与本库准则6 同频的治理线梳理（连 #30 互补） |
| 69 | **HSI（Hierarchical Self-Improvement）** | 单作者（Tailin Zhou，无机构标注） | arXiv 2608.08466（2026.08） | **[有意思]** 任务族专属可热插拔 harness（Trinity 的 per-task-family 引文）——与 #50 构成正反读：任务族分 harness vs 能力分 harness（#50 表3 受控显示后者更优）；单作者无机构降档 T3 |

### 第四梯队：轴外谱系参照

| # | 论文 | 机构 | Venue | 一句话核心 |
|---|------|------|-------|-----------|
| 33 | Constitutional AI / RLAIF | Anthropic | arXiv 2022.12 | 自我批评-修订闭环+AI 反馈训练——"以自身反馈当训练信号"工业先驱（轴外：对齐方法本体） |
| 34 | AlphaTensor | DeepMind | Nature 2022.10 | RL 发现矩阵乘新算法——工具层自动算法发现先驱（AlphaEvolve 谱系前传） |
| 35 | NAS/AutoML 谱系 | 多机构 | 2017-2019 | "自动设计模型"的上一代自动化浪潮及其平台期——RSI 预测的历史对照组 |
| 36 | Altman 系列文章（Reflections / The Gentle Singularity） | OpenAI | 博客 2025 | 头部厂商"AI 已在改进 AI"叙事——立场样本非证据，待时间检验 |

## 与姊妹库的关系

- [data-mixture-insights](https://github.com/liujin99/data-mixture-insights)：数据配比（训练环节的数据决策）；本库：自改进闭环（AI 改进 AI 的所有环节）。
- **连接点**：自动配比搜索（Data Mixing Agent / DomainPilot 线）本质是流程层 RSI 的实例；配比库的 proxy 可靠性结论（rank non-invariance、小 proxy 不可自证）在本库第 6 条准则中同构复用。
