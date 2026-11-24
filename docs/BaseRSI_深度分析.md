# BaseRSI（harness 增益免训练内化进基模权重）深度分析

**总表条目**：#101｜arXiv 2611.24578（v1 2026-11-18）｜T2 **[高价值]**（轻评估⑦→**深读坐实**）｜深读批次⑲（2026-11-24，全文 HTML + 附录；与 #106 配对读——「信号源生死件」双证之正路）

**【一句话】**回流轴第四报名且路线新：**递归 harness 内化**——外环基模自己进化 harness（Holm 校正 leave-one-out 接受测试，8 代 269 候选只收 2 条），内环**同一模型当自己的教师**（带条目 vs 原请求），把 harness 造成的**逐 block 激活差**（ReGap：纯前向、无从输出回传的梯度）回归写进每 block 少数 key-value 神经元（ReKey：位点/强度自测）。Qwen3.8-27B 十个视觉推理基准 ID+11.7/OOD+6.1 全胜 prompting/zoom-in/harness 进化/微调，**编辑后的裸模型超自己带着 harness 的版本**；跨 4 个基模每基准都升；**45.5 GPU 小时全场最少且零 API token、只改 0.23% 权重**。

## 1 出处与身份
- **11 人，6 机构学术线（坐实，机构待核已修正）**：Mingda Zhang（CUHK-Shenzhen）/Yuanhao Dong（NUS）共同一作/Qiang Huang/Rui Qian（复旦）/Yanjin Li（UIUC）/Zhenyu Liu（墨尔本）/Xiao Huang（上交）/Zihao Yang/Jian Wang/Pinjia He/Xiaoying Tang（CUHK-Shenzhen）——**CUHK-Shenzhen+NUS+复旦+UIUC+墨尔本+上海交大**。
- **释放**：repo beita6969/BaseRSI 开源；v1 2026-11-18 无 venue；LLM 使用声明：Claude 只做润色排版（诚实分级）。
- **定位**：对三挑战的正面进攻（Figure 2 谱系）——① harness-bound 增益（每次请求都付 context 税、harness 拿走增益归零）②黑盒监督（蒸馏/RL 信号从输出穿回每 block：慢+内化噪声）③不精确编辑（预设位点的编辑 miss 目标+伤通用能力）。**BaseRSI=把 harness 进化、权重编辑、内化统一进一个递归环**。
- **发现路径**：轻评估批次⑦→深读队列②→用户裁定「继续深读」。

## 2 机制（§4 三件套）
- **外环（harness 进化）**：基模从自己失败提出候选条目；接受测试=**leave-one-benchmark-out**（增益在任一基准剔除后仍显著才收，Holm 阈值+等 token 预算配对+sign-flip p 值）——8 代 269 候选**只接受 2 条**（一个 finer view+一个 counting procedure，接受率 0.7%——接受纪律极严）。被收条目在不相干问题上验证过、给内环一个「verified target」。
- **内环（同模型自教）**：教师=带已接受条目的同一模型，学生=原请求的同一模型；目标=on-policy KL（期望跑在被编辑学生自己的响应上）；scope 外 c(q)=q，同一目标要求编辑**别动基模其他行为**。
- **多轮自响应重拟合**：编辑改变学生响应→covariate shift（模仿学习经典问题）→在被编辑学生自己的响应上反复重拟合；**每轮标签都用未编辑基模计算，所有轮的标签描述同一目标、可池化**；重拟合替换旧编辑、最小化全部轮损失之和（Prop 1：某轮拟合的 gap 不大于任何单线性 readout 在全部响应上的最小正则 gap+随轮收缩的 margin）。
- **ReGap（机制）**：g_l=F_l(教师态带条目)−F_l(教师态不带条目)——**同一 response states 相减=逐 block 的直接效应**，不同 block 的 gap 互不重叠（Prop 2：把每 block 的 gap 都加到学生激活上=输出与教师完全一致）；gap 是「block 与教师距离的**精确下降方向**」（沿 gap 走一步消除分歧；negated/shuffled/换 block 的 gap 全不行）；**全部来自前向，没有从输出回传的梯度**。
- **ReKey（算法）**：①位点测量——把学生 linear-attention 层输出 patch 成教师的，测消除分歧的比例 η：**高→写 MLP 的 KV 神经元；低→view 类条目写 patch embedding**（重采样 kernel K'=BᵀK 使学生把原 patch 嵌成教师上采样后嵌入）；②读取——teacher-forced 前向给每 block 标注 (MLP 输入, gap) 对；③写入——ridge 回归+r 方向截断→每 block r+1 个 SwiGLU KV 神经元（fitted key 携带条目 scope 到未见请求）；④强度——held-out 响应上线搜索选剩余分歧最小的 step。

## 3 主结果核验（Table 1/2 + RQ1-5 逐行）
- **Table 1（Qwen3.8-27B，3 runs mean±std，5 ID+5 OOD 视觉基准）**：BaseRSI ID Avg **72.1**（base 60.4，+11.7）/OOD Avg 70.7（base 67.4）/总 **70.7**（base 64.5，+6.1）——**十基准平均全场最高**；BabyVision +21.6/VLMs Are Biased +16.4/VisuLogic +10.4；**超两个闭源参照**（Claude Haiku 5.5 总 65.6/GPT-6 Luna 57.4）。
- **SFT/GRPO 在同开发问题上几乎不动**（63.9-65.1）——**增益在 harness 学到的东西里，不在拟合开发问题上**；harness 系方法（GEPA/ACE/Meta-Harness/SkillOpt/RRSI）63.9-68.3。
- **内化>留请求（RQ3 核心读数）**：编辑后的裸模型答原始请求**在全部十基准超过**同模型带 harness 请求；scope 内两者打平、**scope 外学生占优**（教师请求无程序、学生权重里带着程序+key 读自己的状态——「privileged information 训练学生」优势）；**编辑合成不需重拟合**（每处在 base 权重上各自拟合）且**合成增益>各自之和**；三轮自响应重拟合就够（第四轮无增益），**在教师响应上重拟合多少轮都无增益**——增益来自「重放谁的响应」。
- **ReGap 消融（RQ4）**：**蒸馏同教师（输出监督）增益不到 ReGap 一半**——即使只训 BaseRSI 写的那批神经元也一样（信号要从输出穿回每 block，几百条响应不够）；**同批神经元上，对逐 block gap 做梯度训练≈闭式解，输出 loss 恢复零——增益来自信号位置，不来自优化器**；两态直接差当信号更低（「逐 block 单独效应」的分解必要）；null edits（同神经元/key/norms、随机 readouts/values）ID 平均不变——增益在拟合的 readouts 里。
- **ReKey 消融（RQ5）**：预设位点/层/强度全部低于测量版；view 条目写 MLP 丢大部分增益（测到 patch embedding 是对的）；mean gap 无回归只加一点点；**fitted key 的 scope 干净度：scope 内 32% 响应引用程序、scope 外 0.5%**；constant key 两边都降。
- **成本（Table 6，H800 80GB）**：**BaseRSI 45.5 GPU 小时=全场最少且最准**（vs harness 方法 51.4-186.6 小时+0.14M-34.0M API input tokens；SFT 230.9/GRPO 159.3 小时）；内化只跑前向+每 block 一次线性求解，**无 backward pass、无 API call**；procedure edit **只改 0.23% 权重、其余 bit-identical、每处可单独移除**。
- **跨模型（RQ2）**：同一循环从 Gemma-4-31B/Qwen3.6-27B/Qwen3.5-9B 原始权重起跑——**每个模型每个基准都升**（内化的条目部分不同）。

## 4 深读金子（摘要外）
- **金子① 「信号位置>优化器」的受控证明（RQ4 最硬）**：同教师同学生同神经元，梯度训练朝逐 block gap 走≈闭式解、朝输出 loss 走=恢复零——**监督信号插在哪一层，比怎么优化更重要**。监督位家族（#100/#102/#152/Keel）的机制级证据：监督位不是工程选择，是增益的上游决定因素。
- **金子② gap=精确下降方向+Prop 2 的可验证性**：逐 block 直接效应的数学（同 response states 相减）让「教师在 block l 教了什么」变成一个可检验的量（沿 gap 一步消分歧、错向/错位全失败）——**RSI 文献里少见的把「教学信号」本身做成可测量对象**。
- **金子③ 0.23% 权重+bit-identical 其余+可单独移除**：增量式权重更新的正面原型——**多处编辑合成进同一 checkpoint 不需重拟合**（vs 连续微调的灾难遗忘问题域 #68）；对姊妹库：中训练多轮配方实验如果走权重侧，「每处改动独立可移除」是可继承性的结构保障。
- **金子④ on-policy replay 的干净对照**：学生自响应三轮 vs 教师响应任意轮——「编辑改变了谁的分布，就要在谁的分布上拟合」；**教师响应再多也不解决 covariate shift**（对配比线：代理档训出的配比结论，换档后要在新分布上重估——同一课的权重侧表述）。
- **金子⑤ 接受纪律的统计形态（外环）**：leave-one-benchmark-out+Holm+sign-flip p 值+等 token 预算配对——269 收 2 的 0.7% 接受率——**#103「选择掷硬币」的解法示范**：选择难就别靠选择，靠统计验收门槛+verified target（连 #91 PACE/#53 撤走纪律的 acceptor 家族新形态）。
- **金子⑥ scope 携带读数**：32% vs 0.5%——fitted key 把条目效应限制在适用的请求上，**编辑不外溢**（连 #102 编辑越权审计 573 次全清的对照面）。
- **金子⑦ 45.5 小时成本账**：内化=前向+线性求解，无 backward 无 API——**「免训练」在这里是字面意义**（对姊妹库：1.5B CPT 验证档如果借鉴 ReGap 思路，harness/prompt 增益的内化成本远低于梯度训练）。

## 5 库内连线
- **#93 TinyAstral（蒸馏回流对照）**：ReGap 的 output-only 消融=蒸馏路线在同一教师/学生/神经元上的直接对照——**回流轴内部首组分差读数**：蒸馏（信号在输出）<ReGap（信号在每 block）。
- **#102/#107 权重自改线**：回流轴三路对照成型——#93 蒸馏/#101 内化（harness→权重，本篇）/#102 外编辑（bug→权重）；#107 Keel 找信号源=ReGap 找信号位置的先驱同族。
- **#68 HCL/forgetting**：金子③（0.23%+可单独移除）=「增量更新必遗忘」假设的正面反例（前提是编辑取自己的神经元）。
- **#103 Resolution Hypothesis（同批对照②）**：金子⑤（Holm 接受测试）=选择问题的统计解法；反馈信号质量谱系（#106 无信号必败→#103 噪声信号随机→本篇精确信号可内化）——**谱系三站齐**。
- **#100 RSI-Prime**：监督位家族（decision-level masking）→本篇 ReGap（block-level gap）——**监督位越贴目标行为、增益越大**的同族证据。
- **#152 RSIBench-Data**：机制四件套的「验证信号内嵌」成员——ReGap 的 gap 就是「内嵌的、可判定的检查」的权重侧形态。
- **姊妹库 data-mixture-insights**：金子③（增量可移除=配方可继承性的结构保障）+金子④（on-policy replay=跨档重估）+金子⑦（内化成本账）——配比线读数已递接口文档。
- **Q1/Q6**：增益可检测才可接受（外环）；教学信号位置决定内化成败（Q6 的「验证器」在权重域的形态）。

## 6 诚实边界
**视觉推理域（VLM）、27B 级**——文本/代码域未测；接受条目只有 2 个（view+counting）——**方法有效性证明≠增益规模证明**（269 候选 99.3% 被拒：外环瓶颈在 proposer 还是接受门槛，论文未分析）；3 runs std 但无多次独立进化 run（连 #103：τ 方差未报）；闭源参照只 2 个且非同档；「privileged information」上限=教师水平（harness 增益是天花板）；无 venue、v1；repo 开源但成熟度未核。

## 7 遗留（深读新设）
- 269→2 的接受率分解（候选质量分布 vs 门槛严格度）——proposer/acceptor 归因。
- 文本/代码域迁移（视觉域的 patch embedding 机制不直接平移）。
- ReGap 与 #93 蒸馏在相同 harness 条目上的跨论文复算（回流轴对照的第三方版）。
- 内化条目的寿命（harness 拿走后增益保留多久/多少轮后衰减）。

## 8 裁定
**T2 [高价值]（深读坐实）**。加分：回流轴路线新（harness→权重的 training-free 通道）+机制数学完整（Prop 1-3）+消融干净（蒸馏/null/预设/重拟合四组）+跨 4 基模泛化+成本账全场最优+诚实边界自提。不加 T1：视觉域单基准族+2 条条目的增益规模有限+无多次进化 run 方差+无 venue、v1。**升级触发器**：文本/代码域复现；269→2 接受率的归因分析；venue 落地；第三方复算（连 #93 对照）。
