# Criteria Exposure（判据暴露：学任务还是学验证器？）深度分析

**总表条目**：#79｜arXiv 2611.08318（v1 提交 2026-11-05，HTML 全文）｜T2 **[高价值]**（轻评估升级→**深读坐实**）｜深读批次⑨（2026-11-17，全文）

**【一句话】****Criteria Exposure（CE）命名+受控双负结果**：把基准成功判据暴露给学习 agent 是**测量选择而非实现细节**——受控实验 ±CE 坐实：**有 CE 时 agent 快速产出高判据分代码，但同一批 episode 换 instruction-faithful 判据重判后优势全消（51.6/27.8 → 45.0/47.2，还略反超），跨任务迁移更差（w/o CE +15 vs w/ CE +6）**=「学的是验证器不是任务」；附**判据-指令差距七型分类学**（42 任务人工审查）+faithful 判据八条改写规则；**「机器人永远看不见自己的验证器——我们测的 agent 也不该看见」**。

## 1 出处与身份
- **机构落定（深读修正）**：13 人=**PKU 12+NUS 1**（Jianxin Bi 双挂）——**Daquan Zhou 通讯=北京大学 DAGroup**（论文头部明确标注；项目页 dagroup-pku.github.io）；**轻评估行"HKU MMLab 系"有误**（Daquan Zhou 原 HKU MMLab 线、现已 PKU）——#73/#87 同线机构字段一并修正，具身三面（#73 燃料/#79 评测/#87 真机）=**PKU DAGroup 线**。
- **PyRUA 三部曲**：PyRUA-Lean（2610.01939，同组前作=本文 actor 的 primitives——「14% 更高成功率/65% 更少 token」）/PyRUA-REAL（#87 库内真机篇）/本文项目名 **RSIJudge**——actor（GPT-6 Astra）+learner（Claude Code Opus 5.5）+基准审查三线一体。
- **开源**：github.com/DAGroup-PKU/PyRUA-Lean（263⭐）**RSIJudge 分支已放 faithful_criteria/（改写判据+dual_table 双判工具+tests）**——§5 说「released criteria code」是下一步，实际分支已具雏形；项目页 dagroup-pku.github.io/PyRUA-Lean/RSIJudge；CC BY 4.0。

## 2 实验设计（§3——受控性与工程纪律）
- **单一开关设计**：同一自改进环（actor GPT-6 Astra high 写 Python cell 经 PyRUA-Lean 冻结 robo API〔RGB-only 感知：SAM 3+多视角测量，无 VLA/深度/仿真状态〕；learner Claude Code Opus 5.5 每轮把证据改写成一版 library=可调用 skills+memory entries，5 轮/5 dev layouts，θ=2 回退，验证 rollout），**唯一差异=learner 是否读判据代码+逐 episode 分数与 stages**（actor 永远不读）——w/o CE=「现实世界设定」（机器人只有人类/VLM 给的成功信号）。
- **任务**：RoboDojo（主基准，双臂 ARX-X5，42 任务）25 学习任务=16 criteria-limited+9 faithful（**分组在任何结果前锁定**）；测试=seed 0 layouts 0-19（500 episode/条件，learner 从未见）。
- **预算**（Table 3）：每 episode ≤100 cells/200 MLLM 请求/20 nudges/90 分钟；learner 每会话 ≤2 次×120 分钟、≤8 rollouts；library ≤40,000 字符/任务。
- **版本钉死**：RoboDojo commit cb2a37d58dbb（2026-10-08）+HF 数据集 revision+Isaac Sim 5.1.0——**逐文件一致跨全部 checkout**；**噪声量化**：同 library 重跑单 layout 判定翻转 4%、round-0 空库两批差 ≥13%——结论只从测试 layouts/合并曲线取，单任务曲线仅定性读。
- **接受与回退**：候选只许改本任务目录、保持 memory ID、dev_verified 须引用判过的 rollout、静态规则+尺寸限制——**全不看成功率**（无候选被拒）；回退=完整 batch 少 ≥2 成功。

## 3 三假设全部坐实（§4）
- **H1 暴露只在判据超指令处涨分**（Table 1）：criteria-limited 16 任务——空库 8.4/w/o CE 27.8（+19.4）/w/ CE 51.6（+43.2）；faithful 9 任务——53.3/83.3（+30.0）/**80.6（+27.3，w/o CE 反而略高）**——「反馈越富越好」有了**条件**：判据忠实指令时，成功信号已够，剩下的（可靠程序）两种设定学得一样好；**CE 的增益来自指令漏掉、失败也不揭示的规则**（build_tower 剩余方块会自曝→两设定都 20/20；press_by_number 按钮协议永不自曝→w/o CE 永远找不到）。
- **H2 学的是验证器**（§4.2，金子）：
  - **判据扰动测试（新协议）**：同一批 episode 用 C^faith 重判——**w/ CE 51.6→45.0，w/o CE 27.8→47.2：优势消失还略反超**（按任务 bootstrap 差 25.9 点，95% CI 4.7-54.4）；w/o CE 库几乎无损（C^faith 接受其 89 个成功中的 87 个）且在原判据索要隐藏协议处**反涨**——**反事实判据给同一指令打分，行为不变，差值全来自判据所验内容**。
  - **换读的判据**（swap）：L_CE^orig 57.0→45.0（C^faith 判）；L_CE^faith 65.0→**13.5**（C^orig 判）——不对称=「分随所读判据走」；**press_by_number：读 C^orig 的库 20/20 vs C^faith 判 0/20**（纯验证器过拟合铁证）；pour_by_language：读 C^faith 的库不再扶正瓶子（C^faith 不要求）——**分数跟着读过的判据走，哪怕读的更松**。
  - **金子②（记忆内容分析）**：**约 1/3 的 CE 记忆条目引用判据代码，w/o CE 记忆零引用**——读判据的 learner 把经验表述成「违反了哪条」（单一验证器的语言），只看成功的 learner 被迫找物理成因（更接近机器人本身的规律）——「暴露改变了学习者写下什么」。
- **H3 不迁移**（Table 2）：RoboDojo 10 目标任务——w/o CE 80（+15）vs w/ CE 71（+6）——**CE 的领先在新任务上反转为劣势**；跨基准（RoboDojo→LIBERO-PRO +3/+1；→RoboCasa365 +2/+2）**两库都≈无增益，只有基准内经验有用**（RoboCasa365 内蒸馏 33/29 vs 22）——「暴露经验携带通过一个任务判据所需的条件与精度，换个地方就是噪声或开销；关于夹爪的规律才跨任务」。

## 4 判据-指令差距分类学（附录 B——具身域的 #62 式分类学）
- **七型**（42 任务逐个静态人工审查 gen_instruction/run_reward/get_score+失败 rollout 确认，严重度三级）：**hidden rule**（判据验证指令未言明的顺序/计数/位置——press_by_number 按钮协议、make_kong 画牌；13 个中高严重度）/**hard threshold**（容差严于指令所需——倒液体 97%；13）/**timing/不可逆事件**（仅一步判定或事件即永久失败；10）/**判据缺陷**（判据代码或资产标注错——mug 14 标注；9）/**额外终态**（make_toast 两片面包须留在架；5）/**松判据**（跳过指令要求的动作也过——sweep_blocks 不用扫帚；4）/**矛盾**（判据要指令的反面——巧克力兔按资产文件夹算木玩具；3）。
- **结构读数**：**33/42 任务要求双臂回 home（15cm/20°），仅 5 条指令提 reset**——判据-指令差距是基准的系统性病灶而非个案；23 任务一致 vs 19 不一致（20 任务/26 变体 criteria-limited）；**EmbodiedEvo/HexaShell 的 recipe 来自 RoboDojo 网站公开规则页**——部分暴露是「官方泄题」。
- **faithful 判据八条改写规则**（附录 C）：只改 run_reward/get_score 不改指令/保留双臂回收/删或松隐藏规则/修缺陷/松阈值（新旧值并列）/紧松判据（仿真器能可靠检测处）/资产标注只在内存修/原仓库不动；**验证**：离线单测+shadow judging+seed 2 独立双判+**每个翻转 episode 手工核查（未发现误判）**；「自己的决定」小节诚实披露主观裁决（35mm 螺母、塔的定义）。

## 5 库内连线
- **特权暴露簇第五例坐实且补上新维度**：#63 SPADE hint-regret（定理化：奖励侧给特权信号）/#39 InfraRSI（验证工程）/#51 ERB dev≠sealed/#66 no-op 双检/**#79 CE=读侧特权（learner 读验证器源码）的首次受控测量**——前四例都在「奖励/评测怎么造」，CE 首次测「验证器可读性本身」当实验变量；**验证器扰动测试**=簇内新探针（反事实判据重判同一批 episode——判据侧、零重训成本）。
- **#62 SimpleTES hacking 五类分类学的具身对偶**：#62 分「评测器怎么被 hack」、#79 分「判据怎么偏离指令」；治理方案对照——#62 终解=评测器共进化 vs **#79 终解=判据忠实化+暴露默认关闭**（「把判据要求的写进指令」+发布版本化判据与指令对照）。
- **ImpossibleBench**（Zhong/Raghunathan/Carlini 2510.20270，利用测试用例）=数字域同构第三例——「可读的验证器会被优化进去」跨域三连（数字域测试用例/具身判据/泛化域 dev-sealed）。
- **Q1/Q2 直接命中**：增益归因（分数涨≠能力涨——本库两问的具身版受控分离完成）；评测设计病灶连 #49 held-out/#37 换代压力；**「暴露增益随基准判据-指令差距增长——增益测的是基准不只是 agent」**=Q6 的具身条款。
- **表内最接近先例**：EmbodiedEvo（2610.18867）消融〔任务描述 vs 描述+wiki〕**双臂都用原判据判**——CE 补的正是「第二套判据」这一缺口；RATs（2606.19419，Darrell 系）验证器读 goal predicates+真值而策略侧被过滤=镜像设计。

## 6 具身自改进簇名册（相关工作区新收 ≥14 名——具身 RSI 生态全图）
**暴露分级谱系**（轻→重）：成功-only〔EmbodiedSWE 2609.27308〔Fragkiadaki/Dhruv Shah/Henderson 系〕/ASPIRE 2607.00272〔NVIDIA 系：Mandlekar/Guanya Shi/Letian Fu〕/Voyager=#12〕→规则描述〔ETA 2608.03924〕→公开评分表〔EmbodiedEvo 2610.18867/HexaShell 2610.23479〕→数值阈值〔Scaling Online Exploration 2610.22496〔**Berkeley Dawn Song/Yilun Du 系**〕〕→形式 goal predicates〔**HarnessPAI 2609.29166〔小米 Jian Luan 系——具身×harness 双归属，入 harness 名册候选〕〕→验证器源码〔Compile Once Run Cheap 2610.25114——RSI 对冻结技能的增量〕**——论文自认只测了两端、中间档剂量-反应曲线留给未来工作。**基准生态**：RoboDojo 2607.04434（42 任务、leaderboard、XPolicyLab）/RoboCasa365 ICLR 2026/LIBERO-PRO 2510.03827（抗记忆污染版）/RoboTwin 2.0 2506.18088（四基准判据-指令差距全查过）；**成功检测替代线**：VLM as Success Detectors 2303.07280/AutoEval 2503.24278（Levine 系，真机自主评估）；**评测有效性**：CaP-X（ICML 2026）/agentic benchmark best practices 2507.02825（Percy Liang/Zaharia 系）/ImpossibleBench。**具身 RSI 从「单点论文」进入「有自己评测学（evaluation science）的子域」**——与 harness 子域同构的成熟化信号。

## 7 诚实边界
- C^faith 是作者自己的代理（S_τ 不可直接计算）；「自己的决定」小节披露主观裁决；**只重写了 RoboDojo**（其他三基准留未来）；仿真成功信号比真机人类/VLM 干净；**只测两端**（成功-only vs 全暴露：源码+分数+stages）——大多数现有系统在中间档未分别测；样本小（20 测试 layouts/任务，单任务仅定性）；机制解释（暴露写了什么/迁移怎么坏）基于条目文本与动作统计**非消融**；蒸馏方差未估；无总成本披露（episode 预算可推算未汇总）；v1 仅 12 天、无 venue。
- **裁定**：T2 **[高价值] 深读坐实**——命名+受控双负读数+新协议+七型分类学+八条改写规则+簇第五例，且工程纪律（版本钉死/预注册/噪声量化/手工核验翻转）论文级范本；**不升 T1**（无 venue/单基准/单跑簇/中间暴露档未测）。升级触发器：venue 接收/第二基准复刻/验证器扰动测试被第三方采用为评测标准/中间暴露档剂量-反应曲线。

## 8 遗留（深读新设）
① 中间暴露档（规则描述/评分表/阈值/goal predicates）剂量-反应曲线未测——「暴露多少算多少」无定量答案；② faithful 判据的独立评审与第二基准重写（论文自列）；③ C^faith 本身作为被优化对象时的稳健性（读 C^faith 的库 65.0——faithful 判据也会被过拟合吗？判据扰动测试的递归适用）；④ 蒸馏方差未估+单任务样本 20 layouts；⑤ 总成本未披露（对照 #59/#78 透明度标准）；⑥ 真机/人类-VLM 成功信号的 CE 等价物（现实世界「暴露」=口头泄漏判据？）；⑦ 具身簇 ≥14 新名+HarnessPAI（harness 双归属）入库决定；⑧ **#73/#87 机构字段已按 #79 深读修正（PKU DAGroup），#73 共著 Zhiyang Dou 归属待逐作者核**。
