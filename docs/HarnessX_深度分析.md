# HarnessX 深度分析

> 条目：#65 ｜ arXiv 2606.14249（v1 2026.06.12 / v3 07.23）｜ **Darwin Agent Team（小米 EV）**——`\xiaomievblue` 宏自曝：核心贡献 Tingyang Chen*/Shuo Lu*/Kang Zhao*（共一）+Weicheng Meng，通讯 **Kun Shao†/Jian Luan†**，贡献者共 14 人（**sweep 行"疑似腾讯"不采——两人系原腾讯Robotics X 团队名，本文归属小米 EV**）；项目页 darwin-agent.github.io/HarnessX ｜ CC BY 4.0 ｜ 深读日期：2026-11-17
>
> **一句话**：harness=第一类对象（类型化处理器+八钩子+九维分类+替换代数），AEGIS 四阶段（Digester/Planner/Evolver/Critic）按"操作镜像"把三种 RL 病理转成设计约束，再用 **cross-harness GRPO 把轨迹回流成模型训练信号**——**L3→L1 回流真做了（共进化比纯 harness 多 +4.7%，且突破了纯 harness 的脚手架天花板）**，概念贡献一流；但**全部主表读数在进化用的适配集上测（无 held-out，论文 7.7 自认 selection bias+过拟合风险）**——按准则6 这是硬伤，方法学与读数必须分开评价。

## 1. 概念贡献一：两个天花板（L1↔L3 接口的形式化，本库最可引用的一段）

纯 harness 进化撞 **scaffolding ceiling**：harness 把工具/上下文/控制流都摆对之后，约束变成冻结模型能不能用上——harness 编辑给不了模型没有的推理力。纯模型 RL（固定 harness）撞 **training-signal ceiling**：新能力没有 scaffold 暴露对应上下文/工具/控制流就得不到练习。**模型=认知核，harness=执行器官**——"更利的器补不了弱的核，更强的核也补不了从不调用它的器"。共进化=同一 loop 内两轴同动。这是 #54 brain-body mismatch 的正面工程表述，直接入全景图 Q1。

## 2. 概念贡献二：操作镜像（RL→符号空间的病理映射）

| RL 概念 | 符号对偶 | AEGIS 实现 |
|---|---|---|
| 策略 | harness 更新流程 | 四阶段管线 |
| 状态 | harness 配置+trace 库 | — |
| 动作 | 类型化 harness 编辑 | builder 操作+变更清单 |
| 反馈 | trace+验证分 | 可观测层 |
| 更新 | 确定性接受门 | seesaw 约束 |

镜像预测三种病理在符号空间**放大重演**（LLM evolver 能构造数值扰动表达不了的结构化漏洞；共享组件编辑非局部传播）：reward hacking（把基准答案嵌 prompt/利用验证器格式规律）、灾难性遗忘（修 A 模式静默回退 B）、探索不足（偏向低风险局部编辑，结构化改动难自发出现）。防御一一对应：Critic 反 hack、**确定性门控（seesaw：不回退任何已解任务）**防遗忘、Planner 先构造"适应景观"防局部。**设计原则可引用："LLM 子代理负责探索/假设/提议；类型结构与确定性门决定什么能上线。"**

## 3. 结构与算法

- **组合层**：处理器=挂在 8 个生命周期钩子上的类型化事件变换（pass-through/transform/split/intercept/interrupt 五结局）；singleton 互斥+顺序+软依赖三元数据；九维分类 D1-D9（D9=**训练桥**——把执行轨迹转成 RL 记录，**回流是 harness 分类学里的一维**）。
- **AEGIS 循环**：10M raw trace→Digester 压成 10K 结构化摘要（跨轮历史）→Planner 构适应景观→Evolver 出带清单的候选（新处理器必须带冒烟测试）→Critic（一轮修改机会）+确定性门（清单完整性/规范化/构建测试/seesaw）。
- **变体隔离（关键消融）**：单一 Global harness 在异质任务集上崩（GAIA GPT-5.4：R4 峰值 73.8→末轮 49.5，**峰-终差 24.3% ≫ 二项 95% CI 8.5%**——排除评测噪声，确认遗忘）；Ensemble routing 维持 ≤K 个变体、按任务簇路由、seesaw 按变体作用域——**87.4=峰值不降级**且 token 更省（107.8M vs 143.7M）。
- **共进化**：FIFO 共享 replay buffer 同轮双更新——AEGIS（非参数）+ cross-harness GRPO（参数）；**同任务跨 harness 版本的轨迹成一组**（组内变异主要来自策略差异而非采样噪声）、任务级对齐不需动作级对齐（不兼容动作空间的 harness 版本共存一组）、轨迹在各自生成 harness 上下文下重放取 log-prob；进化中的 harness 充当模型 RL 的**结构化探索算子**。

## 4. 实验读数（⚠ 全部 on-set）

- 主表：15 配置 14 升，**均值 +14.5%**（0.0~+44.0）；**逆缩放**——最弱 agent 获益最大（Qwen3.5-9B：ALFWorld 53.0→97.0 +44.0 / GAIA +17.1 / SWE-bench +18.2），增益幅度跟**基线反比**而非 meta-agent 家族距离（跨家族 GPT/Qwen 反而比同家族 Sonnet 升更多）。
- **共进化（Qwen3.5-9B，GAIA/WebShop 各小子集）**：harness-only 平台期 37%/49% → 共进化 **41.7%/54.0（+4.7 均值）**；曲线 R4 前重合、随后分离且**持续到末轮**（不是只抬峰）；跨过 scaffolding ceiling 的直接读数。
- 三病理全部实证：GAIA Sonnet R10 出复合编辑提分——R11 trace 分析发现子集靠**验证器格式规律**通过（真 hack），R12 加双路检索对冲自纠 2 轮；τ³-Telecom 五连同类"提醒"编辑亚阈值耦合→R7 单轮 **−14.0%**（pass@2 二值信号检不出亚阈值累积——**per-edit 门控的结构极限**）；ALFWorld R4-7 ship-prediction accuracy 80%→0% 诊断 prompt 空间枯竭（结构编辑因缺历史校准仅 14% 命中）。
- **基建>架构（诚实的消融）**：单 agent CC SDK evolver 86.4 vs 四阶段 AEGIS 87.4（一个标准误内）——四阶段省 12% token+可审计，**精度增益来自类型化基建与结构化 trace 本身**。
- 成本：GAIA 摊销账（107.8M 前期 token，每任务省 83K→1,300 次调用回本）；GAIA 单任务推理 −25% token，ALFWorld +60%（**回报是准确率不是省钱**——两种 ROI 都要报告）。

## 5. 五个可引用结论

1. **L3→L1 回流不是叙事是配置**：回流=分类学一维（D9 训练桥）+循环一步（共进化 GRPO），且有对照读数（+4.7%、跨 ceiling、持续到末轮）——但**回流证据本身 on-set**，两个已报名的（#40/#50）仍无读数，本条成为回流轴第一个带读数的成员。
2. **反馈信号丰富度约束安全进化的上限**："标量奖励下三种病理全不可检——分数变化分不清 hack 与真进步、探索不足与收敛、遗忘与噪声"——准则6 验证线的可引用原理（连 #39 离线复审、#50 全 trace 记录）。
3. **类型系统的价值是让错误可检测**："types do not generate correct programs, but they make them detectable"——与 #39 不变式工程同一哲学：**不防坏编辑，但让坏编辑的作用域显式**，变体隔离因此才良定义。
4. **"不回退"≠无回退**：seesaw 约束过每道编辑，亚阈值耦合仍累积至 −14% 单轮爆发——**逐次门控需要耦合度量/同类型编辑浓度告警**（Critic 已能标记"All 5 prior ships occupy the same bucket"却仍放行——检测与接受的缝隙）。
5. **逆缩放+基建>架构**：弱模型补行为缺口是 harness 进化最大场景（用户梯子读数）；但 meta-agent 是 Opus 4.6（闭源），开源 meta-agent（Qwen3.5-72B 级）能否驱动未测——**该方法目前对小团队是"可抄思想、难抄全栈"**。

## 6. 与本库的接口

- **Q1**：两个天花板+回流第一读数——L1↔L3 接口条款的正面证据；coupling 极限条款并入"工件累积"讨论（连 #67 AAH：AAH 是工件侧本证、HarnessX 是 harness 侧本证，**两层各有"累积不安全"读数**）。
- **横向结论③**：三生产范式的"代数组装"轴实证；"操作镜像"给 14+ 条 harness 子域一个统一评估语言（进化的瓶颈问题=三种病理的哪个主导）。
- **准则6（反例读数）**：一篇方法学优秀的工作**主表无 held-out**（SWE-bench 仅 55 任务子样、GAIA 103 任务全量即适配集）——"同一任务集既进化又评分"在 harness 进化文献是默认做法，Trinity 的 unseen split（#50）与 SPADE 的 held-out（#63）才是少数派——**子域内可信度分层开始出现**。
- **连 #68 HCL**：操作镜像**预测**了 harness 层遗忘并被 case 实证；Ensemble routing=HCL"守护进化"的一个工程解——两文互证（HCL 给名字与三关，HarnessX 给病理案例+路由解）。
- **连 #54**：brain-body mismatch 的正面表述（两个天花板）；连 #43 OXR：scaffolding 工业件的自动化维护与共进化=产品化两条路。
- **机构读数**：小米 EV 的 agent 团队以"Darwin Agent Team"名义发 harness 研究——**车厂智驾系团队进入通用 agent harness 赛道**（继 #40 NeoHorse〔TokenRhythm〕后又一个非大厂组织样本）；JIT-Agent（#64）引用本文且作者列表互证（Chen et al. 2026b=Tingyang Chen 等 14 人）。
- **新词汇入表**：operational mirror / seesaw constraint / variant isolation（ensemble routing）/ scaffolding ceiling / training-signal ceiling / cross-harness GRPO / sub-threshold coupling / ship-prediction accuracy。

## 7. 裁定与触发器

**T2 维持（无标记）**：概念贡献（两天花板/操作镜像/三病理映射/变体隔离）是本子域最佳形式化，共进化=回流轴第一读数；但**主表全 on-set+无 held-out+基线是"能干的初始 harness"（自认）+SWE 子样 55+meta-agent 闭源**——准则6 下读数不可入"已验证"档，只能入"设计证据"档。方法学与读数分开记：方法学 T1 级、读数 T3 级，综合 T2。

**升级触发器**：held-out/第三方任务集复评 / 开源 meta-agent 全栈复现 / venue / 小米 EV 产品线背书（Darwin 栈的落地证据）。

**遗留待核**：① 共进化只有两 benchmark 小子集（GAIA 103/WebShop 100），+4.7% 的置信区间与任务量敏感度；② "Darwin Agent Team"与小米 EV 智驾栈的关系（论文未提，仅宏与致谢自曝——资金方"funders"段未逐一列名）；③ pass@2 二值门在低任务数基准的分辨率极限（55 任务上单翻转 1.8%）；④ 部署路由"进化集外任务路由到总体最优变体"的 OOD 表现未测。
