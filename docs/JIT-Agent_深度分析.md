# JIT-Agent 深度分析

> 条目：#64 ｜ arXiv 2608.25593（v1 2026.08.26 / v2 09.03）｜ **JIT-Agent Team：核心 Guibin Zhang/Leo Lu/Fangzhou Xie，通讯 Wangchunshu Zhou/Chuanrui Hu/Yafeng Deng/Shuicheng Yan——NUS+EverMind AI+NTU**（Guibin Zhang/Wangchunshu Zhou=#54 CoEvo-RSI 同生态确认；Shuicheng Yan=Skywork/SkyAGI 系）｜ 代码 github.com/bingreeky/JIT ｜ CC BY 4.0 ｜ 基于Qwen3.6-27B 训练（JIT-Agent-27B）｜ 深读日期：2026-11-17
>
> **一句话**：**第一个专为 harness 生成而训练的模型**（Model-as-a-Harness）——用 AOT/JIT 范式区分把 harness 优化从"优化持久工件"改成"按任务实例即时合成"；四模块可执行协议+13 种子 HarnessFactory+三段训练（定制 SFT+DPO / 修复轨迹 / Evo-GDPO 在线进化）；**18/18 配对全升、DeepSeek-V4-Flash+JIT 在 DeepSearchQA 85.1 超 GPT-5.6 的 76.0，且 6/6 设置 token/成本全场最低**——"harness intelligence 是可训练、可迁移、与模型 scaling 正交的能力维度"主张的正面实证。

## 1. 范式区分：AOT vs JIT（时间维度进 harness 分类学）

现有 harness 优化（含 #65 AEGIS、ADAS/AFlow/DGM 谱系）共享**Ahead-of-Time 假设**：把 harness 当持久工件在经验流上优化，指望它泛化到未来任务——部署分布稳定同质时成立。但"合适的 harness 不只域依赖，是**实例依赖**的"（宽搜索任务要并行取证、终端任务要精简串行 ReAct、深研任务要对证据的工作记忆、NL2Repo 要文件系统当状态机）。JIT 路线：训练一个生成器，见到任务当场合成 harness，任意现货 LLM 上执行——**把搜索成本摊销进权重**。与 #65（代数组装）/#66（模块分解）构成三维：**时间（AOT/JIT）×组合（代数/模块）×生成（进化一个/训练生成器）**。

## 2. 方法

- **四模块协议**：harness=(记忆, 规划, 动作, 能力编排) 四元组，运行时依赖序 M→P→C→A；不可变事件史+可变控制器状态交互；无显式规划器也可类型一致（null directive）。协议把异构 harness 映射进可比坐标（ReAct/Codex/OpenCode/ROMA/AOrchestra 全可表达）。
- **HarnessFactory**：13 个代表性 scaffold 按统一协议重实现（ReAct/Plan-and-Execute/ReSum/Flash-Searcher/GAM/MemoBrain/AggAgent/OAgent/AgentFold/HiAgent/DeepAgent/ROMA/AOrchestra）=种子库，既测协议表达力又当训练素材与进化初始种群。
- **三段训练**：Stage I 任务条件定制（冻结教师生成协议合规 harness→SFT+按 reward/latency/cost 三通道偏好 DPO，偏好保留条件=reward 升且两效率轴不降其一严升）；Stage II **修复监督**（失败生成+结构化诊断〔编译错/接口错/运行时异常〕配对，仅保留两轮内修好的轨迹——把可靠性变成可学能力）；Stage III **Evo-GDPO**（在线：候选须**超越当前档案前沿**才得分，三通道组内独立归一防尺度淹没，reward 主导+效率仅在不降 reward 时激活；保守保留规则=match-or-exceed reward 且严格改进一个前沿维度才入 bank）。
- **两种推理**：static（并行生成 n 个 harness 只执行选中的——候选多样性不增加 rollout 的轻量 test-time scaling）；**streaming**（跨任务保留 harness bank，按 Stage III 规则更新，**生成器部署时冻结**——测试时进化=档案进化而非权重进化）。

## 3. 实验读数

| 维度 | 读数 |
|---|---|
| 主表（9 基准） | **18/18 配对全升**：GLM-5.2 均值 74.3→81.4（max +20.2 DeepPlanning-Travel 62.8→83.0）；DSv4F 均值 66.5→72.1（DeepSearchQA 76.2→**85.1** vs GPT-5.6 76.0；DeepPlanning-Shopping 59.1→**83.9 +24.8**）；JIT+GLM 九列领先七列 |
| 对照成熟运行时（固定主干） | DeepSeek-V4-Flash/Qwen3.6-Flash × Claude Code/Codex/OpenCode/Hermes/NanoBot：**4/6 性能最佳+6/6 token/成本最低**；xBench-DS 82.0 @212K token/$0.039 vs NanoBot 78.0 @527K/$0.075——**成本降 48% 同时升 4 分**；"没有一个固定 harness 跨任务全胜" |
| Pareto 前沿 | 两 benchmark 前沿均含 JIT 点：DeepSearchQA 上比 NanoBot +7.1 分且便宜 48%；AgentIF（Qwen3.6-Flash）**严格支配所有固定 harness**（升 4.9 分同时把最低成本从 $0.223 压到 $0.078） |
| 跨家族迁移 | 6 主干（DeepSeek V4/Qwen3.6/Mimo-V2.5）×4 基准 vs ReAct：**24/24 全胜**，族级均值 +4.2/+5.8/+6.0——生成器跨模型族迁移，不是给单一主干补丁 |

## 4. 五个可引用结论

1. **"harness intelligence 可训练"成立（三段配方+全迁移读数）**：定制/修复/进化三种能力全进权重，27B 生成器给 9B-Flash 级执行器配 harness——**生成器/执行器解耦的两级栈**是本子域第一个端到端可抄架构（用户梯子读数：训练 27B 生成器难、**用 27B 生成器服务任意小执行器易**——与 #65 meta-agent 须 Opus 级相比，本路线把 meta 成本一次性摊销）。
2. **任务适配 harness 买的是效率不是轨迹长度**：6/6 最便宜且 4/6 最强——"task-adaptive scaffolds improve inference efficiency rather than purchasing accuracy through longer trajectories"——**反"加 token 换分"的行业默认**（连 #50 推理成本读数）。
3. **修复轨迹是可靠性工程的可训练形态**：失败生成→结构化诊断→两轮内修复的模仿学习——把"生成的 harness 必须能跑"从验证门变成训练信号（与 #65 冒烟测试、#63 验证门同一问题域，三文三种解法：**JIT 训进权重 / HarnessX 门在管线 / SPADE 丢进池前验**）。
4. **AOT/JIT=子域的第三个分类轴**：横向结论③"改进对象轴化"再添时间轴——实例级适配 vs 分布级优化不是优劣是两种摊销策略（搜索摊销进进化循环〔AOT〕vs 摊销进权重〔JIT〕）；streaming 模式下 JIT 仍有 AOT 式档案积累——两范式可叠加。
5. **保守档案更新=防漂移设计**：严超前沿才写入（reward 主导、效率不激活除非 reward 不降）——#67"工件累积不保证增益"的正面工程回答，与 #68 HCL 三关提交同族。

## 5. 与本库的接口

- **横向结论③**：三生产范式深读收官——**进化一个（#65 AEGIS 全息案例）/训练生成器（#64 本条配方）/代数组装（#65 组合层）**全部实证确认，轴化归类从假设升为已核读数；"first"自称不采（ACE/DGM 谱系在前），但"**第一个训练出的 harness 生成器**"成立。
- **Q1**：harness bank=部署期外部记忆的又一形态（记忆单位=harness 配置，对照 #67 任务工件/#63 环境记忆）；Evo-GDPO 前沿超越奖励=自生成燃料的有效性信号设计（连 #63 regret——两文都以"超越当前自身"为奖励核心）。
- **用户 3B-A1 梯子**：生成器/执行器解耦直接可用——小执行器+JIT 型生成器（或 AOT 档案）比"训大模型"便宜一个量级；24/24 跨家族迁移说明 harness 资产不锁主干。
- **连 #54**：同一 NUS 生态连发 CoEvo-RSI→JIT-Agent——Guibin Zhang/Wangchunshu Zhou 双署名，生态分工：CoEvo 打共进化机制、JIT 打 harness 生成器化。
- **交叉引用网**：引用 #65 HarnessX（作者表互证小米团队）、ROMA/AOrchestra/RLM（递归系）——harness 子域的引用网正在闭合（#60-#69 内部互引密度上升=子域成型信号）。
- **新词汇入表**：AOT/JIT harness / Model-as-a-Harness / harness intelligence / Evo-GDPO / harness bank / 修复轨迹（repair trajectories）/ 实例依赖性（instance-dependence）。

## 6. 裁定与触发器

**T2 维持**：配方完整+18/18+24/24 读数干净+成本证据细致；生态（NUS+EverMind）多线连发。减分：① **训练任务来自既有 agentic 基准+合成任务，与 9 个评测基准的分离度未明说**（泄漏面待核——准则6 关键问题）；② "超 GPT-5.6"限于两 benchmark 且为系统级对比；③ 教师模型未具名（"frozen stronger teacher"）；④ 无第三方复现、无 venue；⑤ 生成 harness 的运行安全（sandbox 边界）仅以协议约束，未见审计。

**升级触发器**：训练-评测分离度披露 / held-out 任务集复评 / venue / 开源全配方可复现（repo 现为 MIT 代码+权重情况待核）。

**遗留待核**：① 训练任务清单与评测基准重叠度（哪些 benchmark 的任务族进了 Stage I/III 训练分布）；② 教师身份与蒸馏链；③ 13 种子库对"协议外" harness（如 #65 九维中 D5/D6/D7 的执行环境/评测/安全维）的表达力上限——JIT 四模块偏认知侧，安全与沙箱维薄；④ EverMind AI 在其中的角色（公司通讯 Yafu Deng 列通讯作者）与 License/权重开放范围。
