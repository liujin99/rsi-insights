# AntOmniEvo（蚂蚁自动进化框架）深度分析——基建审计型

**总表条目**：#42｜开源框架 ant-research/AntOmniEvo（2026-09-15 建，Apache 2.0）+论文 **Mara Chain**（arXiv 2609.35855，v1 2026-09-25）｜T2 **[高价值]**（轻评估维持→**基建审计坐实**）｜审计批次⑬（2026-11-17；repo+姊妹库+采用证据+论文读数四线）

**【一句话】****厂商进化引擎的工程兑现度审计全部通过——七组件 1:1 落地、演化前后工件全释放、真外部信号存在（复现咨询/bug 报告/HF 收录请求），且论文头对头以 +20pp 级胜我库 harness 簇两员（AHE/Meta-Harness）**：Mara Chain 的核心主张「**弃用候选=信息浪费**」——propose-evaluate-select 惯例把未过验收线的候选扔掉，导致后续提案重访同失败模式；改为**保留拒绝候选+沿单血统用累积证据精炼**（链深 d 有界+Pareto 过滤 Top-N）——AppWorld 585 任务全指标最佳（89.9 vs GEPA 83.3/ACE 83.9），**val 0.8 达标只用 GEPA 34.5% 的 rollouts（3,280 vs 9,514——「65.5% fewer」精确坐实）**；TB2.1 71.9% vs AHE/Codex/Kira 51.7%/Meta-Harness/OpenCode 49.4%。

## 1 身份与释放（repo 审计）
- **主库**：1,230⭐/129 forks/Apache 2.0；**持续活跃**（近 100 commit：09 月 21+10 月 47+11 月 6，最后 push=审计当日）；无 releases/tags；6 贡献者（jacklv111 59 主导=内部开发，31 PR 内部为主）。
- **结构 395 文件**：`antomnievo/` 210（**与论文「七可插拔组件」1:1**：system/=runner、evaluator/、dataset/=data instance、optimizer/、proposer/、evolution_algorithm/、store/=CandidateStore）+**visualizer 116**（变更谱系可视化=「每编辑归因到失败证据」的仪器化）+docs 25（八篇双语全set：quickstart/features/extensibility/when-to-use/system-design/**workspace-artifacts**/**checkpoint-resume**/visualizer）+tests 18（candidate_store/scheduler/token_budget/**pareto_selection**/prooser/agent_lifecycle/diff_dirs/validate/pi_retry）+skills（`eddy-antomnievo-experiment`：九 recipe 的完整操作手册——蚂蚁 eddy coding-agent runtime 露出）。
- **姊妹库 AntOmniEvoDatasetsAndScripts**（11-05 建，master，1,738 文件/9.2MB）：appworld 1,574（submodule）+terminal-bench 76+retrieval-test 85；**演化前后工件全释放**（`prompts/empty_skill` 基线 vs `ds-1slot-with-mara`/`ds-2slots-with-mara` 完整 SKILL.md+references+scripts 目录〔对应论文 slot 配置注记〕；TB2.1 `artifacts/` 演化后 kira 工件；MuSiQue default+evolved pipeline specs）；musique vendored、TB2.1 按文档自行获取。**⚠️ 无 LICENSE 文件（仅 LEGAL.md）+README「Clone」节泄漏内部 alipay git 地址**（工程卫生瑕疵）。
- **论文身份**：6 作者——Ant Group 3（Yubin Lyu/Weixing Mei/Yinan Wu）+北京智能博弈与决策重点实验室 1（Fu Li）+北京国防科技创新研究院 2（Jiawei Fei/Yang Zhao）——**非纯蚂蚁**；v1 无 venue；**AI use statement：框架本身与 AI 助手共同开发**（OpenCode CLI+内部 CLI，Kimi-K3/GLM-5.2，人指导迭代）——狗粮式弱自指（非权重级）。

## 2 论文主读数核验（abstract 声明逐一对表，全坐实）
- **AppWorld（585=168 normal+417 challenge）**：Mara Chain 89.9/83.9/76.7/59.0 **全指标最佳**；GEPA 83.3/67.9/69.3/44.6、ACE 83.9/69.6/74.1/55.4、SkillOpt-Lite 70.8/51.8/68.3/46.0、empty 48.2/35.7/33.3/19.4。**效率**：val 0.8 达标 3,280 rollouts vs GEPA 9,514=34.5%（「65.5% fewer」精确）；墙钟 11.9h vs 35.8h（3×）；终值 0.87 vs GEPA 平台 0.805。
- **TerminalBench 2.1（89 任务，系统模型 GLM-5）**：**71.9% vs AHE/Codex/Kira base 51.7%（+20.2pp）vs Meta-Harness/OpenCode 49.4%（+22.5pp）**——精确坐实。**头对头胜我库 harness 子域簇两员**（AHE/Meta-Harness 均为 #60-#69 sweep 成员）。
- **MuSiQue（4-hop 检索 DAG）**：test nDCG@10 +0.104/Recall@10 +0.131 vs 手写管线；val +0.134/+0.210。**「被优化系统不含 LLM」的轴外泛化证据**（DAG+节点脚本+prompt 文件当工件）。
- **消融（金子级）**：Mara Chain 增益随难度单调增长——Normal 平（86.9=86.9）、Challenge +4.8/+7.2pp、TB2.1 **+14.6pp**（57.3→71.9）、MuSiQue 撤链 −0.034；**机制协同**：full 2,356 rollouts 达 0.79（终 0.87）vs w/o MC 3,480（终 0.81）vs w/o Top-N 5,388——「单机制任何可比预算都达不到 full 终值」。
- **跨模型**：优化后 skill 对 empty 基线 +40.3pp（GLM-5）/+32.7pp（DeepSeek-V4-Pro）/+23.4pp（Qwen3.5-397B-A17B）——**工件跨模型迁移，harness>模型线再证**（连 #57/#86/#52）。

## 3 深读金子（摘要外）
- **金子① 可靠性阶梯+证据触发的表达升级（引用级）**：工件 schema 内建 prose→script→extension 排序（d(a)∈[0,1] 定性先验：prose 可被忽略/script 确定但可不被调用/runtime extension 到达即确定）；**「rollout 证据表明意图修改被绕过时，proposer 可替换/增强为更高可靠性表示」**。案例研究实证：refinement 血统从 prose 建议升级为 script，「最终候选行为由可执行代码而非建议性文本强制执行」；对照组（w/o MC）停留在 prose 级编辑。**=L1→L2/L3 内化动态在优化器内部微缩上演**——优化器自发发现 scaffolding ceiling（连 #65）并自发爬梯（连 #53 技能内化）。
- **金子② 「弃用候选=信息浪费」**：reject 从终止信号变为**谱系节点**（保留 rollout 痕迹/残余失败/结构化分析/变更史，沿单血统精炼同一 minibatch）——Q1 燃料条款新证据：**失败候选含后续优化所需信息，弃掉=重访同失败模式**（连 #63 失败记忆供难度/#78 ruler 保留）。
- **金子③ 变更谱系归因的仪器化**：每候选/运行/分析/变更日志持久化 CandidateStore+断点续跑+visualizer——**准则 6 审计三件套的工程化落地**（每次编辑可追溯到失败证据）——vendor 级别的可复算基建。
- **金子④ 头对头校准 harness 簇**：AHE/Meta-Harness 被 vendor 框架 +20pp 超越——**子域 sweep 的外部校准点：harness 优化器之间存在代差，子域当时的最优不是终点**（连 #60-#69 sweep 结论注记）。
- **金子⑤ 增益随难度增长**：Normal 平、Challenge +5pp 级、TB2.1 +14.6pp——**「保留精炼」的价值集中在长程难任务**（与简单任务的 readily-diagnosed 条件早期就修完的机制解释自洽）。

## 4 采用证据分层（基建审计核心）
- **真外部信号（三条硬证据）**：① 「MuSiQue 复现咨询」issue（10-30，外部用户真在跑复现）；② 调度器「slot 耗尽双预算」bug 报告（10-17，外部者真读了代码——后修复）；③ **NielsRogge（HuggingFace 员工）10-09 请求核验 evals 上 Papers with Code**（收录流程触发）。
- **fork 波**：129 forks 密集于 10-23~10-27 窗口（爆发式曝光——HN/社媒级传播推测），**非渐进深度采用**；外部 PR 未见；无依赖项目证据。
- **开发形态**：内部主导持续维护（74 commits/2 月，无 releases），文档双语完备，测试覆盖核心机制（pareto/budget/scheduler/store 全有测试）。
- **结论**：**曝光级采用（fork 波+复现咨询）成立，生态级采用（下游依赖/第三方贡献）未到**——1.2k⭐ 为真热度但深度使用证据薄。

## 5 库内连线
- **#41 reef**：配送层基建 vs 引擎的互补定位坐实（reef 送工具、AntOmniEvo 优化工具本身）。
- **#60-#69 harness 簇**：AHE/Meta-Harness 被超越 +20pp——sweep 结论需加「外部校准」注记（子域内部排序≠绝对水平）。
- **#65/#53/#72**：可靠性阶梯=优化器内建 L1→L3 爬梯+谱系归因=审计工程化+每轮 fresh 候选池（Pareto Top-N 截断）——三线在此 vendor 框架中同时落地。
- **#63/#78**：reject-as-stepping-stone=失败信息再利用线（Q1 燃料条款）。
- **#49 Harbor**：TB2.1 Harbor job configs 在姊妹库（同沙箱生态）。
- **Q1**：Mara Chain 平台值（0.87 后停=模型能力+链深 d 双限，论文自列）——收敛侧「平台但不崩」的第三种形态（不似 #37 崩、不似 #53 续涨，**以有界链深主动截停**）。

## 6 诚实边界
- **Mara Chain 单批次诊断可过拟合该批失败模式**（论文自列，跨批 per-instance Pareto 仅部分缓解）；上下文构造/诊断逻辑 v1 手工固定（论文自列）。
- AppWorld/TB2.1/MuSiQue 全自报（姊妹库可复算但需自行拼装：TB2.1 数据集不 vendored、appworld 需 submodule）；「20.5% 相对」等个别聚合口径未逐一对上（分项数字全对）。
- 曝光级≠生态级采用；companion 无 license+内部地址泄漏（工程卫生）；链深 d 有界（未来工作：自适应深度）；proposer token 成本未细披露（未来工作列降本）。

## 7 遗留（审计新设）
- MuSiQue 复现咨询 issue 的官方答复与第三方复现结果追踪（升级触发器）。
- GEPA/ACE/SkillOpt-Lite/AHE/Meta-Harness 五基线是否需在总表补行（现仅 #60-#69 簇内部分在库）。
- eddy runtime/Kira agent（蚂蚁内部 coding agent 生态）公开进展追踪。
- 正式 release/tag 与 LICENSE 补齐后生态采用复查。
- 链深 d 自适应续作（论文未来工作）追踪。

## 8 裁定
**T2 [高价值] 维持（基建审计坐实，不升 T1）**。加分：工程兑现度全通过（七组件 1:1/工件全释放/双语文档/机制测试全盖/持续维护）+论文读数自洽可复算+真外部信号三条+头对头胜 harness 簇+20pp。不加 T1 理由：全自报无 venue、曝光级而非生态级采用、Mara Chain 机制 v1 手工固定、第三方复算未落。**升级触发器**：第三方复现落账/venue 接收/下游依赖项目出现/正式 release+license 补齐。
