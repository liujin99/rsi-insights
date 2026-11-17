# Watchlist 2026Q4

> **来源**：HF Daily Papers 2026-10-17 至 2026-11-15 逐日扫描（关键词 arena/rsi/self-improv\*/recursive/self-evolv/self-play/self-training）+ GitHub 热度补充（2026-11-16 扫描）。
> **收录纪律**：凭标题+热度收录，**未读原文、未定梯队——watch ≠ 参考**（对齐姊妹库纪律：直接参考权=工业实战精选独占；新文未经广泛验证只作分析素材）。评估时逐条读原文→定梯队→入总表；不达标者移出并记录。
> **背景判读**：本窗口 RSI 主题论文密度极高（43 条/30 天），36氪 2026-09"刷屏全网"佐证领域已进入热点爆发期——**噪声同步放大，准则 6（自我验证风险）与证据强度排序的把关作用比平时更重要。**

## 评估进度（2026-11-17 更新）

- **已入库**：RSI Arena→#37（T1）；RSI-Anything→#44、RSIHub→#45（T2）；**The Trinity→#50（T2，2026-11-17 机构调查完成：开放式匿名——品牌匿名但 BibTeX 21 人全披露+一作主页实锤，核心锚点 NUS HPC-AI Lab〔Yang You/Wangbo Zhao〕+CUHK 线；环境/产物/Router 未开源故无标记）**；**EvalResearchBench→#51（T2 [高价值]，2026-11-17 深读入库：Q6 评估环首把尺子，深读文档已建）**；**CUA-Master→#52（T2 [高价值]，2026-11-17：Q1 收敛侧受控对照——self-policy 增益有限，开放世界注入续航）**；**R²Orch→#53（T2 [高价值]，2026-11-17：advantage collapse 命名+零技能评测纪律）/ CoEvo-RSI→#54（T2，2026-11-17：brain-body mismatch 命名+TIMR）**；**Recursa→#55（T2）/ On-Policy Self-Training→#56（T3）/ CLE→#57（T2 [高价值]，经验复利地板读数）/ Trustworthy RSI 综述→#58（T3）（轻评估批次，2026-11-17）**；GitHub 生态 9 项→#38-#41/#42/#43/#46-#49（详见下方"GitHub 生态热度补充"段映射）
- **已读摘要、暂留 watch**：（空——摘要级条目已于 2026-11-17 全部评估入库 #50-#58）
- **深读批次（2026-11-17 第七批）**：**#38 Dream-RSI、#39 InfraRSI 深读完成**（两篇深读文档已建：回放模拟器>语义指导受控对照/验证工程范本/LLM 先验负相关/sign flip 命名）；**Dream-RSI 引用线索落地：Anthropic 2026 = riemann-zeta 页 → 新条目 #59（T2 [高价值]）**——研究版 Claude 把 RH 零点下界 41.6%→67.2%（Conrey/Goldston 实审+形式化验证），Anthropic 线入场 L3 发现层首例。
- **深读批次②（2026-11-17 第八批）**：**#50 The Trinity 深读完成**（深读文档已建）——**修正 46 benchmark=28+18**（旧记 44 有误）；迭代 2-4 仍 +9.4/+6.4（Q1 L3 未饱和数据点+Eleusis 非单调诚实披露）；组织消融/路由逼近 Oracle/模块干预/真机 0→60% 全读数入库；防泄漏纪律与 #39/#51 并列**准则6 三范本**；Kaipeng Zhang 归属闭环=上海创新研究院；v1≈v2 正文零差异。
- **harness 子域 sweep 批次（2026-11-17 第九批，总表 #60-#69）**：Trinity/Dream-RSI 深读相关工作反向定位 10 个名字全部入库——EvoX（Berkeley，#38 的在线元优化对照）/SkyDiscover（Berkeley，ACM CAIS'26 唯一 venue）/SimpleTES（#38 的 162× 对照锚）/SPADE（privileged-hint regret=撤走/特权对照信号簇第四例）/JIT-Agent（NUS，harness 生成器化）/HarnessX（harness-model loop=回流 L1 第三报名）/ModularRSI（benchmark-disjoint 卫生学）/Adaptive Auto-Harness（任务流部署+Q1 收敛侧证据）/HCL（harness-level forgetting 命名）/HSI（单作者 T3）。**横向结论③ 设立：harness 进化=L3 最密子域，引用按轴归类**。sweep 注：A-JIT 为同名无关工作已排除；Meta-harness/PACEvolve++ 记候选未入库。
- **深读批次③：harness 三连（2026-11-17 第十批，#63/#64/#65 全文）**：**#63 SPADE 升 [高价值]**——8 held-out 基准 30B-A3B +8.1 且增益随规模扩大（+5.2→+5.7→+8.1）；消融链金标准（**适应性＞生成器强度**：GPT-5.5 静态池 53.0≈自固定池＜自适应 58.3；**无信号自生成有害**：40.5＜不训 50.2）；hint-based regret 唯一定理化（纳什均衡⇒无提示最优）+规模下限（4B/8B 为负）；corpus grounding 防坍缩（无接地 41 连发同一任务）+learnable share+环境预算 $10 亿/年；机构修正：九机构 19 人（"UT Austin/OpenAI/KAIST"推断不采）。**#65 HarnessX**——**L3→L1 回流真做了**（cross-harness GRPO+D9 训练桥，共进化 +4.7% 跨过 scaffolding ceiling 持续到末轮=回流轴第一读数）；两个天花板命名；操作镜像三病理全 case 实证（Telecom 亚阈值耦合 −14.0%=per-edit 门控极限）；逆缩放（+44.0）；**机构修正：小米 EV Darwin Agent Team（`\xiaomievblue` 宏自曝，非腾讯）**——车厂智驾系入场；⚠️ 主表全 on-set（论文自认）→方法学 T1 级/读数 T3 级。**#64 JIT-Agent**——18/18 配对全升+24/24 跨六主干；6/6 成本全场最低（"买效率不买轨迹长度"）；AOT/JIT=第三分类轴；生成器/执行器解耦（27B 生成器服务任意执行器）；遗留=训练-评测分离度未明说/教师未具名。**三生产范式深读收官确认**+**三种可靠性工程解法对照**（JIT 修进权重/#65 门在管线/#63 丢池前验）+**子域可信度分层出现**（held-out 少数派 vs on-set 默认）。
- **深读批次④：元层双连（2026-11-17 第十一批，#60/#62 全文）**：**#60 EvoX 升 [高价值]**——数学 **5/7 在 100 迭代内超闭源 AlphaEvolve**（圆填充第 8 位小数）、Frontier-CS 172 任务中位 75.5 vs 56.2、系统 6/6 超人类最优、成本 <$1 vs OpenEvolve $15.4、**从随机策略起步仍全胜**；**代码=skydiscover-ai/skydiscover 与 #61 同 org 同仓库**（同生态确认）；**元级信号谱系两端落位**（在线真实窗口 vs #38 离线回放——互补叠加）；停滞检测=第五个廉价诊断。**#62 SimpleTES 升 [高价值]**——单一开源 GPT-OSS-120B **28 开放问题全 SOTA**（lasso 合成演化真实泛化/TriMul 跨 GPU 无重调/SLDBench 10× 外推/Erdős 1955/超 AlphaEvolve V2）；**任务-轴匹配**（数学吃 G vs 内核吃 D）；**轨迹级记账后训练 4 held-out 上尾全升+新 SOTA**=回流轴第二读数（比 #65 干净）；**hacking 五类分类学+评测器共进化终解提案**；**修正：作者 33 人非 17 人**；**#62 点名 #60 评测器可被精度 exploit**——"谁的评测器更耐 hack"首次可比。**三层"学怎么搜"全齐**（学环境 #63/学策略 #60/学预算结构 #62）。深读文档累计 9 篇。
- **深读批次⑤：部署/基建双连（2026-11-17 第十二批，#67 全文+#61 五源交叉）**：**#67 AAH（T2 维持不升）**——首个**开放任务流**部署研究（PolyBench 5,075/CTF-Dojo 261/FutureX 503，同一 Sonnet 4.6：市场 22.2→**80.9**、CTF 37.2→50.2、deepdog 带 92 分=逆人群下注）；Prop 1 regret 分解；**工件法证指纹**（16 条运行 4 条劣于 static；GEPA=零搜索/Meta-Harness="DO NOT SEARCH"/Continual=30 工具——**物种分化首次表格化**）；SkillOS 弃权死亡螺旋分解（丢 248 换 5）；**漂移才是杀手**（Gower 异质≈0 关联、energy distance 漂移→冻结重放落后——"不进化不只是不涨，是贬值"）；诚实边界 T1 级（单历史/计算不匹配/轨迹索引含 outcome 标签）；HITL：Generic 73.8＞Specified 61.5；**#68 撞名更正**：AAH "Continual"=Karten et al.（Chi Jin 组）≠我库 #68——引用必须带 arXiv ID；新 sweep 候选 SkillOS/Karten/A-Evolve；**harness 子域碎片化实锤**（引用簇零交集）。**#61 SkyDiscover（T2 维持）**——CAIS'26=**ACM Conference on AI and Agentic Systems** 新会议、Industrial Spotlight、18 人三共一（一作 Shu Liu=#60 共一）；四组件+可编程控制层=元层接口化；**~2.5K LoC 双 SOTA 算法**；数学 6/8 匹超 AlphaEvolve、系统 6/6 超人类；**生态三文一仓分工划定**（框架文/AdaEvolve 2602.20133/EvoX #60——EvoX 论文不提框架文、互引单向）；SkySynth 合成线=sweep 候选；**基建化蔓延获 venue+工业背书**。深读方法注：#61 无 arXiv 版+ACM 页 Cloudflare——官方 BibTeX+S2+姊妹博客+README 五源交叉。深读文档累计 11 篇。
- **深读批次⑥：sweep 收官双连（2026-11-17 第十三批，#66/#68 全文）——harness 子域 sweep #60-#69 十连深读全闭环**：**#66 ModularRSI 升 [高价值]**——两级信用分配（机制级+行为级）+benchmark-disjoint+2,000 任务人工构建集（no-op 零分反 hack+diff 泛化审查门=卫生学三件套）；**Q1 级受控负结果：修改空间↑→可迁移性↓**（joint 44.19/whole 46.44＜baseline 47.57＜模块化 52.43——**约束产生可迁移性**）；**子域唯一跨演化域+跨基座双迁移**（TB→SWE +2.4、换 GLM/MiniMax 执行 +2.2~3.4）；Pass 3 +5.6＞均值增益（降随机失败）；**附录 A 十一系统对比表**=子域底册（≥22 系统）；机构落定：六机构 15 人（Beihang/Manchester/IQuest/M-A-P/Langboat/Hohai，Ming Zhou=Langboat）；**撞名双 Lin**：AHE（J. Lin，RUC）≠A-Evolve（Minhua Lin，Amazon）。**#68 HCL（T2 维持）**——CL 形式化平移 harness 层（NJU 软件新技术国重+UOW——机构落定；CL 社区入场标志）；四组件版本化+三门提交+**锚点集**（纯 harness 层 replay：仅评测可见、双盲、跨任务冻结）；**首次定量 harness-level forgetting 且 ε 单旋钮可控**（62.98/10.94% vs 61.74/2.64%；ε 扫描非单调：有限容忍最好）；CL 字典翻译表（Memory↔replay/Map↔表征/Router↔架构）；**Opt-HCL：27B 只做候选生成〔4B 在线〕+10.65 且延迟不变**——"强模型经持久 harness 蒸馏、在线不用"（#64 解耦的受控定量版）；Minecraft 50 任务 Static 15 后平台 vs HCL 全完成；GSM8K 65.2→96.0；诚实边界：交互域三门退化为 LLM 裁判/遗忘量级小/"遗忘须与能力获取联读"；**与 #67 构成收敛侧对偶**（不进化=贬值/乱进化=破坏）——复利动态修正观四机制闭环；**子域三引用簇定型**（AAH 簇/ModularRSI 簇/深读簇，桥接节点仅 Meta-Harness/HarnessFix）。深读文档累计 13 篇；十连盘点：4/10 升 [高价值]，Q1 新增条款（元级三轴/任务-轴匹配/回流证据链/燃料三维/两个天花板/复利动态修正观/约束产生可迁移性/经 harness 蒸馏）+Q3 新工具（停滞检测/评测器共进化/五指纹/双距离/泛化审查门/锚点集/ε 旋钮）。
- **轻评估批次②：watch 前五热度收编（2026-11-17 第十四批，#70-#74 凭 abs 摘要）**：**#72 Self-HPO（T2 [高价值]，MSR 系）**——harness-policy 交替共进化（policy 当自己的 harness 提议者+hard tasks 构成下一轮 harness 搜索集）：57.8→64.8（+7.0）OOD 保持=共进化轴第三读数最大单读数；**联合增益＞个体增益之和（超可加性首次明示）**；组件混搭保持；强模型 proposer 再加成（Opus 4.8）=强提议者/弱执行者第三例；对 #54 的正面回答=交替而非跟随。**#74 HarnessPro（T2，清华 THUDM）**——成本轴进目标函数（Pareto lite/medium/heavy 三档）：对 RHO（同一作前作，#66 附录 A 表内系统）+2.0pp 且 −26.1% 双轴同向；label-free（LLM 生成测试）=准则6 新高危面；子域第十轴。**#71 A-RSI（T2，Trento+小米）**——时间轴问题单独成轴（训练日志判衰退刷新任务池、无需验证分数）：「何时换燃料」与 #60「何时换策略」同构=供给侧时间轴三件套；零数据 57.77→61.05。**#70（T3 [有意思]，NUS Show Lab）**——「harness」概念扩散到视频域立场报告+RSI 五层分解跨域同构。**#73 PhysLoop（T3 [有意思]，HKU MMLab）**——LLM 物理知识经 harness 蒸馏成视频模型燃料（跨模型燃料注入）+100K 视频。总表 74 条（11/42/17/4）；Takeaway +#72（27 条）；全景图 Q1 补轻评估②注记（超可加/时间轴三件套/成本轴/跨模态燃料）。
- **轻评估批次③：watch up≥20 七条收编（2026-11-17 第十五批，#75-#81 凭 abs 摘要）**：**#78 Épi（T2 [高价值]，MIT+ND+MSR+OSU+CMU+AI2 大联盟 19 人）**——跨会话持久研究 agent（持久历史+分层记忆+分支独立评审+**固定验证器**）：**120 项记录**（n-queens 84×/**Feigenbaum 四次元 3.6×10⁶× 收窄/Cohn-Elkies d10/11 不尖锐对偶证书**/Tuza 猜想 split 图计算机辅助证明）+**Épi-turbo 同底座超 Codex 5.4% 且成本 64%**；L4 线部署现实性升级（会话间连续研究，连 #59/#67）。**#79 Criteria Exposure（T2 [高价值]，HKU MMLab 系）**——CE 命名+受控负结果：判据暴露下判据分快涨，**判据微扰（指令不变）后优势消失+迁移差=学的是验证器不是任务**；验证器扰动测试新协议+instruction-faithful criteria 修复；特权暴露簇第五例（#63/#39/#51/#66 后具身域落地）。**#76 EmoReward（T2）**——reward traces 蒸馏证据可靠性（实例门控+有界乘数、**不更新奖励模型**）=**训练轨迹元信号奖励侧**（#71 燃料侧/#60 策略侧后第三侧）。**#80 MoLRSI（T2，MIT 系）**——**湿实验验证**：抗生素 hit 15.9% vs 专家 8.7%（同数据同库）；肽-MHC 8 命中 **vs Claude Science 完全失败**（强声明待深读核）。**#75 VibeGame（T3）**——training-free 自进化（骨架/契约蒸馏=经验抽象最弱形态）+实现/评审分离；同底座超一众 harness。**#77 Physical AI RSI（T3）**——增益前传四条件（复利四机制物理域对偶）+「物理域尚未隔离递归增益」诚实负结果。**#81 PhysRSI-eval（T3）**——五轴改进度量+木桶假说+早期反馈预测后期进展（廉价早期信号家族第三成员）。总表 81 条（11/46/20/4）；Takeaway +#78/#79（29 条）；全景图 Q1 补轻评估③注记（验证器扰动测试/物理域递归缺口/奖励侧元信号/L4 持久性）；RSIHub 行补标（早期批次漏标修正）。
- **轻评估批次④：watch 未读池 up≥10 清空（2026-11-17 第十六批，#82-#89 凭 abs 摘要八条）**：**#82 DiscoveryRSI（T2，PKU/BIGAI 系 26 人）**——假设价值操作化（可检验假设=证据落地+可行调查+判定标准+替代解释）+假设树进化搜索编译 **Harbor 任务**（连 #49 同沙箱=基建趋同）；DFM-Bench 四层研究进度测量；**截断后文献对应率 6.3% vs GPT-6 2.5%/Opus 5 2.3%**（新颖性外部时间锚）；4.86→5.39 两轮。**#83 Build, Don't Play（T2，南京大学 Limin Wang 系）**——**「LLM agent 造系统远强于当系统」真机定量**：20 天自建 TavernGym+23 版 TavernAgent，天梯 6,000→~15,000、均名次 1.60/80% 第一**超真服务器前十人类玩家**；自建智能体 91% vs 前沿 LLM 直接游玩 ≤63%、0.03s vs 26s（L1/L3 分工部署级证据）；每断言过验证门+真机闭环。**#86 ParetoRSI-Gym（T2，UC Berkeley Dawn Song 组）**——多域 Pareto 后训练 gym+**每域泛化基准分离能力 vs 过拟合**（准则6 评测制度化）；「harness 与模型同等重要」评测侧再证；**agent 数据选择策略指纹**（连 #67）；+9.2% 相对（+2.3 绝对）诚实。**#88 ACE-RL（T2，Berkeley Sherry Yang 系）**——**交接物（playbook）构造首次被训练**：按下游效应评分，训单模型兼任行动/反思/策展（#55/#52「构造不被学习」缺口补上；#62 轨迹记账平移到工件层）。**#84 ECHO（T3）**——单步自指目标函数+模型当自己的起草者（轴内边缘：自指非迭代环）。**#85 SERA（T3）**——rubric 用验证结局排序训练（学评分反哺解题）；机构不透明降档。**#87 PyRUA-REAL（T3，HKU Daquan Zhou 组——与 #79 同组作者重叠）**——真机经验→可执行技能→脱机部署；8 真机任务 91.1%；HKU 线具身三面（#73/#79/#87）。**#89 Auxiliary Rewards（T3，Georgia Tech Alan Ritter 系）**——等正确轨迹学习价值分离；+1.93（bootstrap CI 统计纪律）。总表 89 条（11/50/24/4）；Takeaway 维持 29；全景图 Q1 补轻评估④注记（造>玩/多域纪律/时间锚/交接物监督/中间信用分配）；未读池剩全部 ≤up 8 边缘条目。
- **深读批次⑦+触发器扫尾三查（2026-11-17 第十七批）**：**#72 Self-HPO 深读完成（深读文档第 14 篇，[高价值] 坐实）**——超可加性精确核验（H2+3.5/P2+3.4/联合 +8.5＞和 6.9）；**SFT 蒸馏 OOD 崩塌**（Opus 轨迹 SFT 双 OOD 跌破 Base vs harness 侧转移双 OOD 全涨——「知识经 harness 转移存活 OOD、经权重蒸馏不存活」）；RL 零成功组 6.5% vs 11-17%（治奖励稀疏）；35B-A3B〔3B active〕69.5≈Gemini 3 Pro；**proposer 反烘焙约束**（机制必须运行时自己算信号）+四类诊断+提交卫生四门（预算阀/空补丁守卫/契约接地/补丁回退）；Resolved −2.2 诚实披露；MSR 6+UVA 2+CMU 1 机构落定、开源兑现（MSR-Orchard/Self-HPO）；Table 5 设计空间分类（HASE=最接近对手）；**相关工作新收 ≥18 名→子域 ≥40 系统**（WHALE/HASE/Evo²/Lin 2605.30621 会改≠会受益/MetaRSI 元-元层/Apple 线四连/RHO=Pan 2606.05922 落定）——Self-HPO=首个跨三引用簇枢纽。**触发器三查**：⑩ 未兑现（Arena 仍 v1、无 battle 数据仓库）；#38 arXiv 仍未挂（二次核验）；⑭ **#59 部分兑现**（论文更新版 08-13+Lean 形式化+过程转录本+到达路径附录公开，模型未公开；方法论细节入库：31M tokens/650 失败想法→60 subagent 分工〔2 核心+13 贡献+30 尝试失败+13 验证+2 写作〕/**下载 54 篇 arXiv 自查新颖性**/「鼓励提示」克服自我怀疑）→ #59 行触发器核验注记入库。
- **未读（仍凭标题）**：Skill^∞ / Evolving with Open Worlds / SIMIT / TinyAstral / TSFLab / RSIGame / More Memory, Worse Agents / 多模态综述（#30 待核版本）等（**轻评估④已收编 up≥10 全部八条：#82-#89，2026-11-17——watch 未读池剩全部 ≤up 8 边缘条目**，按需批量评估或留观察）

**已评估入库**：#37 RSI Arena（2610.33624，up 26）→ T1 [高价值] + README 重点关注①（2026-11-16）。

## A. 通用 RSI 方法/框架（harness/自进化框架线）

| 标题 | arXiv | 日期 | 热度 | 备注（凭标题/摘要级） |
|---|---|---|---|---|
| The Trinity: Capability-Driven Recursive Self-Improvement for Multimodal Agents | 2610.36077 | 10-28 | **up 161** | **已入库 #50（T2，2026-11-17）**：能力树+Real2Sim2Real+harness 树进化；开放式匿名（NUS 生态），环境/产物/Router 未开源——升级触发器=开源兑现/venue/⭐≥1k/复现 |
| Video Generation Meets Agent Harness: Agentic Video Generation, World Modeling and RSI | 2610.40191 | 11-03 | up 77 | **已入库 #70（T3 [有意思]，2026-11-17 轻评估②）**：NUS Show Lab 系立场报告——「harness」概念扩散到视频生成域+RSI 五层分解跨域同构 |
| A-RSI: Adaptive Task Refresh for Zero-Data Multimodal Self-Improvement | 2610.38568 | 10-30 | up 67 | **已入库 #71（T2，2026-11-17 轻评估②）**：时间轴问题单独成轴（训练日志判衰退刷新任务池）——供给侧时间轴三件套 |
| R²Orch: RSI for Agent Orchestration via Self-Play within a Skill-Internalization Loop | 2610.22740 | 11-02 | up 49 | **已入库 #53（T2 [高价值]，2026-11-17）**：CUHK-SZ 系——Recursive Learnability+Skill Withdrawal+advantage collapse 命名，12/12 榜+零技能稳健 |
| Self-Evolving Coding Agents via Harness-Policy Optimization | 2610.22791 | 10-20 | up 48 | **已入库 #72（T2 [高价值]，2026-11-17 轻评估②）**：MSR 系 Self-HPO——交替共进化 +7.0 OOD 保持、**联合增益＞个体之和（超可加性）**、强模型 proposer 再加成 |
| CoEvo-RSI: When Harnesses Evolve, Which Models Should Follow? | 2610.25078 | 10-21 | up 46 | **已入库 #54（T2，2026-11-17）**：NUS 系——brain-body mismatch 命名+TIMR 诊断+双向历史继承；9B 三垂直域超 DeepSeek-V4-Pro/GLM-5.3（自报窄证据） |
| RSIHub: A Composable Framework for Agent-Evolution Methods | 2610.32253 | 10-26 | up 44 | **已入库 #45（T2，2026-11-17 早期批次）**：可组合框架——基础设施型工作（本行补标：早期已入库、此前漏标） |
| HarnessPro: Recursive Harness Self-Improvement with Pareto Optimization | 2610.26770 | 10-20 | up 31 | **已入库 #74（T2，2026-11-17 轻评估②）**：清华 THUDM——成本轴进目标函数（+2.0pp 且 −26.1% 双轴同向）；RHO=同一作前作 |
| On-Policy Self-Training: Data-Free Self-Improvement in LLMs | 2610.32770 | 10-26 | up 31 | **已入库 #56（T3 [有意思]，2026-11-17）**：Google+Ulm——免数据自蒸馏极简配方（特权推理当软验证） |
| InfraRSI: Harnessing RSI for Training Infrastructure | 2611.11558 | 11-10 | up 34 | **已入库 #39（T2 [高价值]，2026-11-17 深读完成，文档已建）**：华为诺亚系+清华 HPC 签名——验证工程范本/LLM 先验负相关/sign flip/多节点死亡区 |
| Recursa: Toward Recursively Self-Improving Foundation Model Systems | 2611.04035 | 11-04 | up 18 | **已入库 #55（T2，2026-11-17）**：阿里 Jingren Zhou 系——结构化知识仓库自经验路线，与 #52 构成正反对照线 |
| ECHO: Efficient Causal Self-Recursion | 2610.24766 | 10-20 | up 15 | **已入库 #84（T3 [有意思]，2026-11-17 轻评估④）**：百度 ERNIE 系——单步自指目标函数+模型当自己的起草者（非迭代环，轴内边缘注记） |
| Self-Improvement with Auxiliary Rewards: Learning from Verified Traces | 2610.41296 | 11-03 | up 10 | **已入库 #89（T3 [有意思]，2026-11-17 轻评估④）**：Georgia Tech Alan Ritter 系——等正确轨迹学习价值分离（下游增益评估纪律；连"验证器=执行器"） |
| Experience-Handoff Reinforcement Learning for RSI | 2611.16409 | 11-13 | up 10 | **已入库 #88（T2，2026-11-17 轻评估④）**：Berkeley Sherry Yang 系——交接物构造首次被训练（下游效应评分） |
| Skill^∞: Recursive Self-Improvement for Agent Skill Optimization | 2610.35434 | 10-27 | up 8 | 技能优化（连 #12 Voyager） |

## B. 评测/基准（L5 测量线）

| 标题 | arXiv | 日期 | 热度 | 备注 |
|---|---|---|---|---|
| Continual Learning Exam: Benchmarking Long-Horizon RSI and Experiential Learning | 2610.33956 | 10-27 | up 35 | **已入库 #57（T2 [高价值]，2026-11-17）**：UCL Jun Wang 系——经验复利首把大规模标尺+硬地板读数（23.3% 全通关） |
| EvalResearchBench: Can AI Agents Build Their Own Feedback for RSI? | 2610.04184 | 10-23 | up 19 | **已入库 #51（T2 [高价值]，2026-11-17 深读）**：agent 自建评测首标准 benchmark——PA≈75%/CCC≈23/dev≠sealed；深读文档已建，遗留 repo 复算核验+第二届追踪 |
| DiscoveryRSI: Recursively Learning to Discover Valuable Hypotheses | 2610.23839 | 10-19 | up 19 | **已入库 #82（T2，2026-11-17 轻评估④）**：PKU/BIGAI 系——假设价值操作化+DFM-Bench 四层测量+截断后文献对应率外部锚（连 Q6/#49 同 Harbor 沙箱） |
| ParetoRSI-Gym: Can AI Agents Improve Language Models Across Multiple Domains? | 2610.34386 | 10-27 | up 11 | **已入库 #86（T2，2026-11-17 轻评估④）**：UC Berkeley 系——多域 Pareto+每域泛化分离（准则6 评测制度化；与 #37 同构问题） |
| Self-Evaluating Recursive Agents | 2610.04902 | 10-20 | up 12 | **已入库 #85（T3 [有意思]，2026-11-17 轻评估④）**：rubric 用验证结局排序训练（学评分反哺解题）——准则6 高危样本的"有锚自评"变体；机构不透明降档 |

## C. 领域应用（L3/L4 具体域）

| 标题 | arXiv | 日期 | 热度 | 备注 |
|---|---|---|---|---|
| CUA-Master: Scaling Computer-use Agents through Open-World Recursive Improvements | 2611.03126 | 11-03 | up 50 | **已入库 #52（T2 [高价值]，2026-11-17）**：MMLab CUHK+THU+HKUST(GZ)+京东；self-policy 注释器增益有限 vs 更强注释器持续增益——Q1 收敛侧受控对照；权重+全流程开源 |
| PhysLoop: LLMs' Physics Awareness for Self-Improving Video Generation | 2611.16748 | 11-13 | up 46 | **已入库 #73（T3 [有意思]，2026-11-17 轻评估②）**：HKU MMLab 系——LLM 物理知识经 harness 蒸馏成视频模型燃料（跨模型燃料注入） |
| VibeGame: Prompt-to-Game with Self-Evolving Adversarial Agent Team | 2610.33106 | 10-30 | up 26 | **已入库 #75（T3 [有意思]，2026-11-17 轻评估③）**：NTU S-Lab 系——training-free 自进化（骨架/契约蒸馏）+实现/评审分离；同底座超一众 harness（连 RSIGame） |
| Toward Recursive Self-Improvement in Physical AI | 2610.29677 | 10-23 | up 23 | **已入库 #77（T3 [有意思]，2026-11-17 轻评估③）**：HKUST-GZ 系立场文——增益前传四条件+「物理域尚未隔离递归增益」诚实负结果 |
| Epi: Towards a Persistent, Self-Improving Agent for Autonomous Research | 2610.24944 | 10-21 | up 22 | **已入库 #78（T2 [高价值]，2026-11-17 轻评估③）**：跨会话持久研究 agent——120 记录（Feigenbaum 3.6M×/Cohn-Elkies 对偶证书/Tuza 部分）+Épi-turbo 同底座超 Codex 5.4%@64% 成本（L4，连 #17/#18） |
| MoLRSI: A Recursive Self-Improving AI agent for Molecular Discovery | 2610.39714 | 11-04 | up 20 | **已入库 #80（T2，2026-11-17 轻评估③）**：MIT 系——湿实验验证（抗生素 hit 15.9% vs 专家 8.7%）+肽-MHC 8 命中 vs Claude Science 完全失败（待核）；连 #23 co-scientist 谱系 |
| EmoReward: Self-Evolving Reward Calibration for Emotional Video Generation RL | 2611.06518 | 11-05 | up 23 | **已入库 #76（T2，2026-11-17 轻评估③）**：reward traces 蒸馏可靠性（门控+有界乘数不更新奖励模型）=训练轨迹元信号奖励侧 |
| Build, Don't Play: Self-Improving LLM Research Agents Develop a Hearthstone AI | 2610.33845 | 10-27 | up 18 | **已入库 #83（T2，2026-11-17 轻评估④）**：南京大学 Limin Wang 系——真机天梯超前十人类玩家+「造系统远强于当系统」 |
| PyRUA-REAL: Robot Exploration and Adaptive Learning through RSI in the Real World | 2611.06768 | 11-05 | up 11 | **已入库 #87（T3 [有意思]，2026-11-17 轻评估④）**：HKU MMLab 系（与 #79 同组）——真机经验→可执行技能→脱机部署 |
| RSIGame: Autonomous Agentic Game Development with RSI | 2609.39045 | 09-24 | HF Daily #10（10-01）/ GitHub 171★ | CUHK-SZ（Biwei Huang 组）；局部探索-诊断-改进+全局饱和检测，经验内化 LoRA（Qwen3.8-27B 超 GPT-5.5 one-shot） |
| TinyAstral: Transferring Self-Evolving Harnesses to Tiny Generalist Manipulation Agents | 2610.41216 | 11-03 | up 4 | harness 向小模型迁移 |
| Evolving with Open Worlds: RSI for Future Forecasting | 2610.29784 | 10-22 | up 7 | 预测域 RSI |
| TSFLab: A Self-Improving Agent-Agnostic Workbench for Time Series Forecasting | 2610.40901 | 11-02 | up 2 | 时序研究工作台 |
| SIMIT: Self-Improving VLMs via Imagination at Test-Time | 2611.00053 | 11-03 | up 5 | 测试时想象自改进（L2 边界） |

## D. 综述/理论/负结果

| 标题 | arXiv | 日期 | 热度 | 备注 |
|---|---|---|---|---|
| Trustworthy RSI in the Era of Large Models: Foundation, Risks, Evaluation and Open Challenges | 2610.35193 | 10-27 | up 23 | **已入库 #58（T3 [有意思]，2026-11-17）**：人大高瓴——与准则6 同频的风险分类综述 |
| Learning the Task or Learning the Verifier? Rethinking Criteria Exposure in Embodied Self-Improvement | 2611.08318 | 11-13 | up 21 | **已入库 #79（T2 [高价值]，2026-11-17 轻评估③）**：Criteria Exposure 命名+受控负结果（判据微扰优势消失=学验证器不学任务）——Q1/Q2 直接相关，特权暴露簇第五例 |
| Where is Physical RSI capability from? An Observational Scaling Law of LLM-as-policy | 2610.26848 | 10-20 | up 20 | **已入库 #81（T3 [有意思]，2026-11-17 轻评估③）**：PhysRSI-eval 五轴改进度量+木桶假说+早期反馈预测后期进展（Q1 相关） |
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
| zhengkid/Dream-RSI（Google/DeepMind） | 1.6k | **#38 T2（2026-11-17 深读完成，文档已建）**（升级触发器=arXiv〔11-17 核验仍未挂〕/venue/第三方复算/⭐≥3k） |
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

## 评估优先级（changelog 同步；2026-11-17 九次更新——harness 子域 sweep 批次完成）

1. **#60-#69 深读排期（✅ 全闭环 2026-11-17）+ #72 Self-HPO 深读（✅ 2026-11-17 深读批次⑦，[高价值] 坐实）** → 2. **深读排期（轻评估升级件）**：#78 Épi（[高价值]：120 记录可复现性/验证器泄漏面）与 #79 Criteria Exposure（[高价值]：受控负结果核验）优先；候选 #83 AutoTavern（真机读数+人工参与度）/#86 ParetoRSI-Gym/#88 ACE-RL → 3. 未读 watch 条目滚动评估（**轻评估④已收编 up≥10 八条 #82-#89——未读池剩 ≤up 8 边缘条目**：Skill^∞/More Memory 负结果线/SIMIT 等，按需批量评估或留观察） → 4. #50/#57 后续读数追踪（Open-Source Plan 兑现/CLE 二届） → 5. 触发器扫尾（2026-11-17 三查已记录：⑩ Arena battle 数据未兑现、#38 arXiv 仍未挂、⑭ zeta 方法论部分兑现——三者继续追踪后续变化） → 6. 增量触发器：harness 子域新论文按轴归类评估（待办⑮ 名册已扩至 ≥40：AHE/TACO/RSEA/WHALE/HASE/Lin 2605.30621/MetaRSI 等，入库决定待做）→ 其余。
