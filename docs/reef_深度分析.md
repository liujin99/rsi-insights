# reef（持续自改进 agent 基建）深度分析——基建审计型

**总表条目**：#41｜GitHub Human-Agent-Society/reef（2026-08-31 建，Apache 2.0，PyPI reef-infra）｜T1 **[高价值]**（轻评估 T1→**审计维持，加基建型限定**）｜审计批次⑭（2026-11-17；repo+配方库+采用证据+在库复现记录四线）

**【一句话】****自改进基建枢纽的审计全面坐实——工程规模与诚实标准都是本库已审计对象中最高的**：2,988 文件（recipes 1,427+tests 702+core 567）/2.5 个月 **901 个 merged PR**/5 releases/30+ 贡献者/7,835⭐+740 forks+512 watchers；配方库 12 条**横跨 L1 权重训练×L3 harness 进化双路径**（gepa/meta_harness/ace/reasoning_bank‖sdft/sdpo/opd/sao/tttd/openclawrl），每条钉死上游 SHA+运行时版本；**meta_harness RESULTS.md 在库诚实复现：TB 30 任务×2 重复，Reef 23/60 vs 上游基线 24/60——噪声级打平且负向亦照记，声明「非基准测量」**——配送层定位（自己不发明算法）+准则 6 级诚实=审计维持 T1 的双支柱。

## 1 身份与工程规模（repo 审计）
- **org**：Human-Agent-Society（2026-03-16 建，5 repos）——先有 CORAL（autoresearch 引擎，1,133⭐，与 org 同日）后有 reef；主贡献者 Zijian Zhou（BobbyZhouZijian，505 commits）；desc：「为人类与 AI agent 协作学习的社会造引擎」。
- **主库**：7,835⭐/740 forks/512 watchers/Apache 2.0/Python 3.12+；**审计当日仍在推送**；5 releases（v0.0.2 09-02→v0.1.3 10-14，节奏稳定）；62MB；Trendshift 徽章（发布脉冲成分坐实）。
- **结构 2,988 文件**：recipes 1,427+**tests 702**（测试/代码比≈1.2:1 于核心——工程纪律）+reef core 567（train 108/service 73/harness 63/inference 47/storage 34/execution 31/dispatcher/observability/artifact/tinker——**推理-反馈-学习-版本化交付全链路模块化**）+docs 167（站点 reefinfra.ai）+tutorials 60+CI 40；**AGENTS.md+CLAUDE.md+recipes/AGENTS.md**（repo 级 agent 开发指令=AI 辅助开发基建化——与 #42 AI use statement 同谱系的工程落地）；SECURITY.md/CITATION.cff/双语 README。
- **协议化+卫星库**：reef-client（08-17，「Reef protocol」stdlib-only SDK——协议被单独形式化）+reef-eval（**08-09，早于主库**——评测基建先行）；社区运营全套（Roadmap issue #25/Office Hours issue #818/Discord+WeChat/X launch post）。
- **2026-11 重构进行中**：control plane+workers 架构（README 明示「重构完成前部分 recipes 在 main 上可能跑不通」——诚实的中断警告）；Semantic Router 集成后又弃用第三方、内联化（外部 PR）。

## 2 配方库逐条核（12 条，两路径分区）
- **L3 harness 进化侧**：gepa（42 文件：archive/components/reflection 全实现+reflection_prompt）/**meta_harness**（含 RESULTS.md 在库复现记录）/ace（**Agentic Context Engineering** arXiv 2510.04618——playbook bullets+Reflector+Curator+**「代码而非模型合并 bullets」**——**=Mara Chain 论文的 AppWorld 基线 ACE 的可运行实现**）/reasoning_bank（ReasoningBank 2509.25140——judge 标记无真值运行+extractor 写记忆条目+作为 Agent Skill 供出）。
- **L1 权重训练侧**：sdft/sdpo/opd（10 月新增，#683）/sao（Single-Rollout Asynchronous Optimization 2607.07508）/tttd（**Learning to Discover at Test Time 2601.16175——=待办㉒ L4 簇的 TTT-Discover，现有了厂商复现**）/openclawrl（OpenClaw-RL 2603.10165——从用户后续行为当奖励训练个人 agent）。
- **模板**：basic（local-sglang/external-provider 双 yaml）/beta。**每条配方=上游论文钉 SHA+方法 python 包+examples**——「算法的配送层」主张被 12 件具体工件坐实；配方从轻评估的 5 条扩到 12 条（+7）。

## 3 深读金子（在库复现记录——准则 6 的 repo 级范本）
- **金子① meta_harness RESULTS.md**：vanilla Terminus 2 基线+4 轮全历史迭代，30 任务×2 重复=60 trials，严格改进才替换：**Reef 20/60→选 iter1 23/60（后续两轮 20/20、第四轮 23 平局保 iter1）；上游 24/60→全程保基线（22/20/21/21）**——复现结果**噪声级且相对上游基线略负**（重评 fresh 重复 22/60 vs 21/60 又略正）；**选择器复盘等价性**（600 条记录回放，Reef selector 与上游 update_frontier 决策完全一致=实现保真度验证）；**钉死一切**（上游 commit SHA/任务 revision SHA/运行时版本 Harbor 0.20.0/LiteLLM/E2B/模型 ID gpt-5.6-luna+proposer gpt-5.6-sol xhigh/原文件 SHA-256）；四重免责声明（「非论文全基准/非未改上游性能参考/30 任务方法复现/本地 runner」）；诚实记录 terminal-loss zero 与 invalid/null verifier reward 保留。**配送层不冒充性能层**——「方法复现≈打平」也照记的 repo 级诚实标准，超过多数论文。
- **金子② 双路径的工件级统一**：同一 runtime 下 6 条 L1 配方+4 条 L3 配方可组合——「框架的框架」从口号变成可检查的目录结构；**Semantic Router 流量切分**（场景跨 served 工件分流量，连 #50 Trinity Router 线）+Tinker 训练组件+Modal 沙箱+HF artifact 仓——集成面覆盖 2026 工具栈。
- **金子③ 采用证据的分层诚实读法**：星标 7.8k≈发布脉冲（Trendshift 驱动，轻评估预警坐实）+**持续层真实**（740 forks/512 watchers/**PyPI 1,594 下载/月**——实际包使用量级≈研究基建早期典型值）；901 merged PR 中近期 100 个的 83 个来自主开发者——**内部高速开发为主，外部贡献少而实**（hanfeiyu 的沙箱系列〔认证流量/AgentENV 后端〕、RickWayne1125 的 LoRA 导出/Semantic Router 弃用、dannyward630 的 SGLang 兼容——都是真实使用碰撞出的修复）；issue 总量 418（社区支持负载真实）。
- **金子④ 评测基建先行**：reef-eval 创建日（08-09）早于主库（08-31）——**先造尺子再造引擎**的工程顺序（连 #51 ERB「评估环优先」判断）。

## 4 库内连线
- **#42 AntOmniEvo**：配送层 vs 引擎的互补对坐实——reef 送算法（12 配方=别人的算法钉 SHA 复现）、AntOmniEvo 优化你的系统（自家 Mara Chain）——**reef 的 ace 配方=Mara Chain 的 AppWorld 基线的可运行版**，两审计批次的对象在基线层相遇。
- **待办㉒（L4 簇）**：tttd 配方=TTT-Discover 的厂商复现——簇成员获得落地载体。
- **#50 Trinity**：Semantic Router 流量切分连 Router 线；**#63 SPADE**：reasoning_bank「无真值自学习」与 hint-based regret 同属自生成信号问题。
- **#78 Épi**：openclawrl 从日常使用信号训练（下一轮当接受/投诉当拒绝）——OpenClaw 生态再+1。
- **#38-#49 GitHub 生态组**：唯一 T1——其余为工具/框架单件，reef 是**横跨 L1×L3 的配送层**，枢纽地位经配方库实证。
- **姊妹库 data-mixture-insights**：reef 的 train 配方线（sdft/sdpo/opd/sao/tttd）=权重训练侧配送——姊妹库「自动配比」线的潜在挂载点。

## 5 诚实边界
- **采用=脉冲重、持续中**：7.8k⭐ vs PyPI 1.6k/月——星标数≠使用量；持续采用看 2027-02 半年线（轻评估触发器维持）。
- **v0.1.x+重构中断期**：control plane+workers 完成前 main 可能破损（自认）；Semantic Router 集成后又弃用第三方（接口不稳定期）。
- **复现记录的边界**（RESULTS.md 自列）：30 任务小集/2 重复无方差报告/「campaign scripts, raw histories, and audit records remain internal」（原始记录未释出）/非 held-out。
- **"first"自称仍在 README**——不采信维持（hermit/RSIHub 同期同域）。
- **无算法自有主张**（by design）——引用它不构成对任何性能主张的背书；T1 地位=基建枢纽+诚实标准，非科学发现。

## 6 遗留（审计新设）
- 2027-02 半年线复查（星标持续曲线/PyPI 增长/下游论文出现）。
- control plane+workers 重构完成后复跑配方库全量核（当前中断警告）。
- reasoning_bank（无真值自学习）与 #79 判据暴露的交叉实验场（judge 标记=判据可读性的又一实例）。
- 姊妹库挂载评估：train 配方线是否进 data-mixture-insights 的配送层清单。
- CORAL（autoresearch 引擎，1.1k⭐）是否独立入库评估。

## 7 裁定
**T1 维持（基建型限定）**。维持理由：工程规模与节奏（901 PR/5 releases/30+ 贡献者/测试比 1.2:1）+配送层主张被 12 配方工件级坐实（L1×L3 双路径）+**在库复现记录的诚实标准=准则 6 的 repo 级范本**（钉 SHA/噪声级打平照记/四重免责）+评测基建先行+协议形式化。限定：T1 地位=基建枢纽+诚实标准（非科学主张，无自有算法）；引用价值=作为 L1/L3 算法的配送层与复现基准。**降级触发器**：重构长期失败/2027-02 半年线采用崩塌；**升级观察**：首篇以 reef 为基建的第三方论文。
