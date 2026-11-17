# Watchlist 2026Q4

> **来源**：HF Daily Papers 2026-10-17 至 2026-11-15 逐日扫描（关键词 arena/rsi/self-improv\*/recursive/self-evolv/self-play/self-training）+ GitHub 热度补充（2026-11-16 扫描）。
> **收录纪律**：凭标题+热度收录，**未读原文、未定梯队——watch ≠ 参考**（对齐姊妹库纪律：直接参考权=工业实战精选独占；新文未经广泛验证只作分析素材）。评估时逐条读原文→定梯队→入总表；不达标者移出并记录。
> **背景判读**：本窗口 RSI 主题论文密度极高（43 条/30 天），36氪 2026-09"刷屏全网"佐证领域已进入热点爆发期——**噪声同步放大，准则 6（自我验证风险）与证据强度排序的把关作用比平时更重要。**

## 评估进度（2026-11-17 更新）

- **已入库**：RSI Arena→#37（T1）；RSI-Anything→#44、RSIHub→#45（T2）；**The Trinity→#50（T2，2026-11-17 机构调查完成：开放式匿名——品牌匿名但 BibTeX 21 人全披露+一作主页实锤，核心锚点 NUS HPC-AI Lab〔Yang You/Wangbo Zhao〕+CUHK 线；环境/产物/Router 未开源故无标记）**；**EvalResearchBench→#51（T2 [高价值]，2026-11-17 深读入库：Q6 评估环首把尺子，深读文档已建）**；**CUA-Master→#52（T2 [高价值]，2026-11-17：Q1 收敛侧受控对照——self-policy 增益有限，开放世界注入续航）**；GitHub 生态 9 项→#38-#41/#42/#43/#46-#49（详见下方"GitHub 生态热度补充"段映射）
- **已读摘要、暂留 watch**：R²Orch（编排层 self-play+技能内化，指出 advantage collapse/单噪评分问题）；CoEvo-RSI（NUS 系〔Guibin Zhang/Ivor Tsang/ Shuicheng Yan〕，"brain-body mismatch"命名模型-harness 协同进化错配）；On-Policy Self-Training（免数据自蒸馏，自生成 QA+特权推理演示）；Recursa（**Jingren Zhou=阿里**——新域自适应靠自己经验不用更强老师）；Continual Learning Exam（大规模长时程 RSI 基准）；Trustworthy RSI 综述（治理线，核对与 #30 重叠）
- **未读（仍凭标题）**：HarnessPro / Self-Evolving Coding Agents / A-RSI / MoLRSI / Epi / VibeGame / PhysLoop / EmoReward / ParetoRSI-Gym / DiscoveryRSI / Skill^∞ / Experience-Handoff RL / ECHO / SIMIT / TinyAstral / Evolving with Open Worlds / TSFLab / Build Don't Play / PyRUA-REAL / 物理域与多模态各条

**已评估入库**：#37 RSI Arena（2610.33624，up 26）→ T1 [高价值] + README 重点关注①（2026-11-16）。

## A. 通用 RSI 方法/框架（harness/自进化框架线）

| 标题 | arXiv | 日期 | 热度 | 备注（凭标题/摘要级） |
|---|---|---|---|---|
| The Trinity: Capability-Driven Recursive Self-Improvement for Multimodal Agents | 2610.36077 | 10-28 | **up 161** | **已入库 #50（T2，2026-11-17）**：能力树+Real2Sim2Real+harness 树进化；开放式匿名（NUS 生态），环境/产物/Router 未开源——升级触发器=开源兑现/venue/⭐≥1k/复现 |
| Video Generation Meets Agent Harness: Agentic Video Generation, World Modeling and RSI | 2610.40191 | 11-03 | up 77 | "harness"概念扩散到视频生成域 |
| A-RSI: Adaptive Task Refresh for Zero-Data Multimodal Self-Improvement | 2610.38568 | 10-30 | up 67 | 零数据多模态自改进 |
| R²Orch: RSI for Agent Orchestration via Self-Play within a Skill-Internalization Loop | 2610.22740 | 11-02 | up 49 | 编排层 self-play + 技能内化（连 L1/L3） |
| Self-Evolving Coding Agents via Harness-Policy Optimization | 2610.22791 | 10-20 | up 48 | 编码 agent harness 策略优化 |
| CoEvo-RSI: When Harnesses Evolve, Which Models Should Follow? | 2610.25078 | 10-21 | up 46 | harness 进化×模型换代交互（连 GEAR 线） |
| RSIHub: A Composable Framework for Agent-Evolution Methods | 2610.32253 | 10-26 | up 44 | 可组合框架——基础设施型工作 |
| HarnessPro: Recursive Harness Self-Improvement with Pareto Optimization | 2610.26770 | 10-20 | up 31 | 性能-成本 Pareto 权衡进 harness 进化 |
| On-Policy Self-Training: Data-Free Self-Improvement in LLMs | 2610.32770 | 10-26 | up 31 | L1 层免数据线（连 #16 Absolute Zero） |
| InfraRSI: Harnessing RSI for Training Infrastructure | 2611.11558 | 11-10 | up 34 | RSI 改训练基建（连 #05 AlphaEvolve 判断——L3 先行） |
| Recursa: Toward Recursively Self-Improving Foundation Model Systems | 2611.04035 | 11-04 | up 18 | FM 系统级递归自改进 |
| ECHO: Efficient Causal Self-Recursion | 2610.24766 | 10-20 | up 15 | 因果自递归（理论线） |
| Self-Improvement with Auxiliary Rewards: Learning from Verified Traces | 2610.41296 | 11-03 | up 10 | verified traces 辅助奖励（连"验证器=执行器"） |
| Experience-Handoff Reinforcement Learning for RSI | 2611.16409 | 11-13 | up 10 | 经验交接 RL |
| Skill^∞: Recursive Self-Improvement for Agent Skill Optimization | 2610.35434 | 10-27 | up 8 | 技能优化（连 #12 Voyager） |

## B. 评测/基准（L5 测量线）

| 标题 | arXiv | 日期 | 热度 | 备注 |
|---|---|---|---|---|
| Continual Learning Exam: Benchmarking Long-Horizon RSI and Experiential Learning | 2610.33956 | 10-27 | up 35 | 长时程 RSI 基准（连 #20 RE-Bench） |
| EvalResearchBench: Can AI Agents Build Their Own Feedback for RSI? | 2610.04184 | 10-23 | up 19 | **已入库 #51（T2 [高价值]，2026-11-17 深读）**：agent 自建评测首标准 benchmark——PA≈75%/CCC≈23/dev≠sealed；深读文档已建，遗留 repo 复算核验+第二届追踪 |
| DiscoveryRSI: Recursively Learning to Discover Valuable Hypotheses | 2610.23839 | 10-19 | up 19 | 假设发现（L4 评估线，连 Q6） |
| ParetoRSI-Gym: Can AI Agents Improve Language Models Across Multiple Domains? | 2610.34386 | 10-27 | up 11 | 多域 agent-改-模型 gym（与 #37 同构问题） |
| Self-Evaluating Recursive Agents | 2610.04902 | 10-20 | up 12 | 自评递归 agent——准则 6 高危样本 |

## C. 领域应用（L3/L4 具体域）

| 标题 | arXiv | 日期 | 热度 | 备注 |
|---|---|---|---|---|
| CUA-Master: Scaling Computer-use Agents through Open-World Recursive Improvements | 2611.03126 | 11-03 | up 50 | **已入库 #52（T2 [高价值]，2026-11-17）**：MMLab CUHK+THU+HKUST(GZ)+京东；self-policy 注释器增益有限 vs 更强注释器持续增益——Q1 收敛侧受控对照；权重+全流程开源 |
| PhysLoop: LLMs' Physics Awareness for Self-Improving Video Generation | 2611.16748 | 11-13 | up 46 | 视频生成自改进 |
| VibeGame: Prompt-to-Game with Self-Evolving Adversarial Agent Team | 2610.33106 | 10-30 | up 26 | 游戏生成+对抗自进化团队（连 RSIGame） |
| Toward Recursive Self-Improvement in Physical AI | 2610.29677 | 10-23 | up 23 | 物理 AI RSI |
| Epi: Towards a Persistent, Self-Improving Agent for Autonomous Research | 2610.24944 | 10-21 | up 22 | 持久自主研究 agent（L4，连 #17/#18） |
| MoLRSI: A Recursive Self-Improving AI agent for Molecular Discovery | 2610.39714 | 11-04 | up 20 | 分子发现 RSI（连 #23 co-scientist 谱系） |
| EmoReward: Self-Evolving Reward Calibration for Emotional Video Generation RL | 2611.06518 | 11-05 | up 23 | 奖励自校准 |
| Build, Don't Play: Self-Improving LLM Research Agents Develop a Hearthstone AI | 2610.33845 | 10-27 | up 18 | 游戏环境自改进研究 agent |
| PyRUA-REAL: Robot Exploration and Adaptive Learning through RSI in the Real World | 2611.06768 | 11-05 | up 11 | 真实世界机器人 RSI |
| RSIGame: Autonomous Agentic Game Development with RSI | 2609.39045 | 09-24 | HF Daily #10（10-01）/ GitHub 171★ | CUHK-SZ（Biwei Huang 组）；局部探索-诊断-改进+全局饱和检测，经验内化 LoRA（Qwen3.8-27B 超 GPT-5.5 one-shot） |
| TinyAstral: Transferring Self-Evolving Harnesses to Tiny Generalist Manipulation Agents | 2610.41216 | 11-03 | up 4 | harness 向小模型迁移 |
| Evolving with Open Worlds: RSI for Future Forecasting | 2610.29784 | 10-22 | up 7 | 预测域 RSI |
| TSFLab: A Self-Improving Agent-Agnostic Workbench for Time Series Forecasting | 2610.40901 | 11-02 | up 2 | 时序研究工作台 |
| SIMIT: Self-Improving VLMs via Imagination at Test-Time | 2611.00053 | 11-03 | up 5 | 测试时想象自改进（L2 边界） |

## D. 综述/理论/负结果

| 标题 | arXiv | 日期 | 热度 | 备注 |
|---|---|---|---|---|
| Trustworthy RSI in the Era of Large Models: Foundation, Risks, Evaluation and Open Challenges | 2610.35193 | 10-27 | up 23 | 可信 RSI 综述——L5 治理线，评估时核对与 #30 的重叠 |
| Learning the Task or Learning the Verifier? Rethinking Criteria Exposure in Embodied Self-Improvement | 2611.08318 | 11-13 | up 21 | "学任务还是学验证器"——Q1/Q2 直接相关 |
| Where is Physical RSI capability from? An Observational Scaling Law of LLM-as-policy | 2610.26848 | 10-20 | up 20 | RSI 能力的 scaling law 观察（Q1 相关） |
| A Cross-Scenario Survey and Analysis of Self-Evolving Multimodal Agents | 2611.14256 | 11-13 | up 3 | 多模态自进化综述（连 #30 待核版本） |
| More Memory, Worse Agents: Error Reproduction and Anti-Persistence in LLM Agents | 2610.27406 | 10-21 | up 6 | 负结果线——记忆反噬，证据排序最耐用一类 |

## E. 相关但非 RSI 本体（arena 基建/邻域，不评估仅记录）

| 标题 | arXiv | 日期 | 热度 | 备注 |
|---|---|---|---|---|
| SO-Arena: Benchmarking System One Models | 2611.15777 | 11-13 | up 138 | 评测基建，非 RSI |
| UserArena: Interactive User Simulation for Long-Horizon Shopping Agents | 2610.30932 | 10-26 | up 55 | 用户模拟 arena |
| Embodied Agent Arena（VLM Robot Generalists 实证） | 2610.00854 | 11-04 | up 24 | 具身 arena |
| Quant-Arena: LLM Agents for Quantitative Trading Research | 2610.30113 | 10-22 | up 6 | 量化研究 arena |

## GitHub 生态热度补充（2026-11-16 扫描；2026-11-17 批量入库）

RSI 已成 GitHub 热词（2026-08 后新建仓库爆发）。**入库映射（2026-11-17 业界收集批次①）**：

| 项目 | ⭐ | 入库 |
|---|---|---|
| Human-Agent-Society/reef（自改进基建枢纽） | 7.8k | **#41 T1**（star 触发器激活） |
| SoMarkAI/OXR（产品化 RSI 后训练） | 3.8k | **#43 T2**（候选观察席） |
| TokenRhythm/NeoHorse（agentic post-training TR） | 1.7k | **#40 T2** |
| zhengkid/Dream-RSI（Google/DeepMind） | 1.6k | **#38 T2**（升级触发器=arXiv/复现） |
| ant-research/AntOmniEvo（蚂蚁） | 1.2k | **#42 T2** |
| Gen-Verse/RSI-Anything（世界模型 RSI） | 1.2k | **#44 T2** |
| lobehub/awesome-rsi（领域地图） | 1.0k | 不编号：分类法与本库 L1-L5 对照可互校，暂记参考源 |
| AetherLabsAI/RSIAgent | 610 | **#46 T3** |
| Infini-AI-Lab/hermit | 527 | **#48 T3** |
| rsi-gear/gear（GEAR，ICL+Duke 系） | 464 | watch 保留（论文 2026-10 preprint，深读后排期） |
| Danielxu0208/rsi_eval（评测 SDK） | 470 | watch 保留 |
| Gen-Verse/ScienceBuddy | 425 | **#47 T3** |
| aiming-lab/RSI-Exam | 198 | **#49 T3** |
| cocacola-lab/awesome-embodied-rsi | 282 | watch 保留（具身 RSI 专题） |
| Human-Agent-Society/CORAL | 1.1k | watch 保留（reef 姊妹项目，未读） |

**判断**：星标速度远超学术采用速度，防发布脉冲条款（准则 4 新文条款）必须严格执行——reef（2.5 月 7.8k）与 OXR（1 月 3.8k）为脉冲嫌疑最大的两个，半年线复核。

## 评估优先级（changelog 同步；2026-11-17 四次更新）

1. **R²Orch**（49，编排层 self-play+技能内化——CUA-Master 完成后顺移至最高）→ 2. **CoEvo-RSI**（46，NUS 系）→ 3. Dream-RSI/InfraRSI 深读（总表待办⑨）→ 4. The Trinity 深读+开源兑现追踪（总表待办⑩）→ 5. EvalResearchBench 遗留：repo 复算核验+第二届追踪 → 6. CUA-Master 遗留：OSWorld 第三方复算+self-policy 对照定量数字核读（总表待办⑪）→ 其余按热度。
