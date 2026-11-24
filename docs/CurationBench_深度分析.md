# Curation-Bench（通用 agent 能否自动化数据策展）深度分析

**总表条目**：#150｜arXiv 2606.04261（v1 2026-06-02）｜T2 **[高价值]**（引用链补扫→**用户点名深读坐实**）｜深读批次⑯（2026-11-24，全文 HTML + 附录）

**【一句话】****数据策展环的第一把专用尺子**：固定模型+固定训练配方+固定评测，只把「数据策略」留给 agent 在终端里迭代（写脚本→提交→训练→看分→改）——通用 agent 十轮就摸到人类基线（59% of full gain@1.5% 数据），但轨迹诊断暴露**执行-研究差距**：agent 卡在配比微调这类廉价局部变体里；强制「引用-实例化-适配一篇先例方法」的重型脚手架才把它推出舒适区，组合出 EL2N 损失选择+噪声过滤策略，**用 1/10 数据量超过 100k 人类基线**。

## 1 出处与身份
- **8 人学术团队（摘要级「机构待核」坐实）**：Feiyang Kang/Hanze Li/Adam Nguyen/Mahavir Dabas（Virginia Tech）/Jiaqi W. Ma（UIUC）/Freic Sala（Frederic Sala，UW-Madison）/Dawn Song（UC Berkeley）/Ruoxi Jia（Virginia Tech，通讯，ReDS lab）——NSF 三grant + Amazon-VT 资助；多机构学术线，无单一公司署名。
- **谱系**：DataComp（Gadre et al. 2023）的数据隔离原则（固定模型/配方/评测，只留数据当变量）+ 交互式终端 agent 循环（对 MLE-bench/AutoML agent 线的补位：此前 agent 基准测「改训练配方」，本基准测「改数据策略」）。
- **释放**：代码+基准 harness 全开源（github.com/feiyang-k/curation-bench），license CC BY 4.0；轨迹/manifest/评测日志逐 commit 存档——**可复算纪律强**。
- **发现路径**：深读 #100 RSI-Prime 参考文献挖掘（第三十六批引用链补扫）；用户点名深读（做中训练数据配比）。

## 2 机制（§2 契约 + §4 脚手架阶梯）
- **任务形式化**：τ=(M₀, D_pool, B, T, E, C)——初始模型/候选池/数据预算/固定训练流程/评测套件/约束；agent 产出数据策略 π，映射 D_pool→D_π（|D_π|≤B），得分 S(π)=E(T(M₀, D_π))。**P1 数据隔离：架构/优化器/训练日程/评测全钉死，agent 唯一自由度=数据**。
- **提交门控**：schema+预算校验→污染审计（SHA-256 精确匹配+词 8-gram 重叠≥0.8 近重复，对八个目标基准；high_risk 强制改了再交）→才启动训练；迭代=一记 commit（脚本/manifest/审计结果/训练输出/评测分/运行笔记全存档）。
- **轨迹诊断四标签**（LLM 标注，Claude Opus 4.7）：**new policy family**（换策略家族）/grounded（有证据链）/effective（同预算同配方下改进）/shallow（无假设的局部反应式调整）——把「怎么搜」变成可审计对象。
- **脚手架阶梯**：open-prompt（只有任务接口）→ Light I「数据策略清单」（可选）→ Light II「论文技能卡」（可选）→ Heavy I Self-Research（强制每轮写观察-假设-预期-最小改动）→ Heavy II Adapt Papers（**强制每轮引用一篇先例方法、解释为何适用、适配成代码**）。
- **主实例**：LLaVA-665K 池（六源：coco 364,100/vg 86,417/ocr_vqa 80,000/gqa 72,140/text_only 40,688/textvqa 21,953）选 10k → LLaVA-1.5-7B 微调（1×A100、1 epoch、vision tower 冻结、lr 2e-5 全钉死）→ 8 基准（VLMEvalKit，judge=Qwen3.5-27B 固定端点）。**附带一个从零预训练实例**：DataComp-Small CLIP 过滤赛道（ViT-B/32 random init、12.8M seen-pairs 预算、8.77M 可恢复池、单 H100、一轮 ~2h 训练+~2h 评测、38 零-shot 任务）。

## 3 主结果核验（Table 2/5/6/19-22 逐行）
- **开源 agent 达人类基线（Table 2，10k/10 iter，8 基准均分）**：base 28.8→全量 665k 微调 37.1；random 10k 31.9±0.3（45% of full gain，10 运行最好 32.5=45%）；ICONS 33.3（54%）/ARDS 33.2（52%）；**Claude Code Opus 4.7 33.7±0.3（59%）**/Codex GPT-5.4 33.3±0.2（54%）/Kimi K2.5 32.8（48%）/Qwen3.5-397B 33.2（53%）。执行可靠性：50+ 会话 500+ 迭代，非外因崩溃 <10 次。
- **脚手架阶梯（Table 5，Claude Code）**：open 33.7（59%，最佳策略=「调源配比：boost GQA/TextVQA」——**最佳结果就是配比微调**）/Light I 33.8/Light II 33.6（**两个轻脚手架都不超过 open 的最佳 34.0**）/Heavy I Self-Research 32.9（grounded 57%→91%、shallow 47%→9%——**更讲证据反而更低分**）/Heavy II Adapt Papers 34.0 均值、**最佳 34.9（63%）**（new family 27%→67%、grounded 100%、shallow 0%）。
- **最佳策略本体**：EL2N 式 top-loss 选择+assistant-loss 噪声过滤的组合（训练动力学家族）——**34.9 超被测 100k 人类基线 ARDS 34.1、逼近 ICONS 100k 34.5，数据量 1/10**；参照系 random 100k=33.7、random 200k=34.0。
- **迭代预算=策展算力（§5.1）**：10→50 iter 平均持续改进无平台（fixed data budget）；Heavy II 最佳 10 轮内找到、余下迭代降方差。
- **改写扩展（Table 6）**：agent 用外部 MLLM 工具改写 10k——Qwen3.5-9B 20-iter 34.7（**71% of full gain**）>Qwen3.5-2B 34.0>Gemma3-4B 33.1——**改写工具选择是主导变量**。
- **DataComp 实例**：agent（Claude Code）超过最强过滤基线 top-30% CLIP L/14（正文「notable margin」；**具体数字只在图 3(b)，正文未给精确值**——已列遗留）。
- **任务泛化消融（Table 22）**：换目标模型/换池（Qwen2-VL-2B 48.1→48.5/Qwen2.5-VL-3B 63.1→63.5/Vision-Flan 31.8→32.2）——**增益缩到 +0.4**（vs 主实例 +1.2）：策展策略对 LLaVA-665K 池结构有过拟合成分。

## 4 深读金子（摘要外）
- **金子①「数据策略≠配比」——八家族分类学（Table 14）**：数据策略=八个部分互不连通的家族——质量/格式过滤、**分布/域（调源配比——用户做的事）**、语义覆盖（聚类多样性）、任务/指令平衡、**训练动力学（按 loss/EL2N/梯度范数选）**、去重/污染、风格改写、生成反馈。**换家族常需要新工具/新代理/新假设（论文原话 partially disconnected）**。这就是「数据策略不就是配方吗」的准确答案：训练配方（optimizer/日程/超参）在 Curation-Bench 里是被钉死的对照组；数据策略是自变量；**配比只是自变量八个维度之一，而且是 agent 最容易 instantiation 的最廉价维度**。
- **金子② agent 卡在配比家族=行为学级读数（Table 3）**：示例轨迹 10 轮里第 5-10 轮全是「COCO=2500, others=1875」型源配比微调，一半被标 shallow；论文命名 **vibe optimization**（计划听着像研究、操作判据未指明）。open-prompt 的最佳策略就是配比微调（59% of full gain 的天花板=这个家族的天花板）。**对姊妹库：配比微调是「最低成本局部变体」，它的收益天花板在本文被实测出来——且换家族（训练动力学）才拿更高分**。
- **金子③ Heavy II 的反直觉三连**：(a) **立即有效迭代率反而降**（20% vs open 30%）——换家族要先付失败成本；(b) 但终分最高=「搜索到达高上限区域」比「局部改进率」重要（论文自评 trajectory diagnostics 的存在理由）；(c) Heavy I（只强制讲证据）**降分**（32.9<33.7）——**讲证据不等于会找方向**；要「把先例方法适配成代码」这种程序性约束才有效。
- **金子④ 脚手架强度=实验变量（E.2/E.3）**：同一 backbone 换 scaffold=不同系统；**论文号召把 scaffold 强度当模型超参一样披露**（open 指令/宽 prompt/技能清单/可执行技能卡/脚本化管线五档阶梯），「重脚手架提高性能但降低自主性——这是要测量的特征，不是要避免的缺陷」。连 #105 AdaObs（观测面）与 #62（预算结构）：**自主系统研究里「约束什么」本身是一等实验变量**。
- **金子⑤ 策展算力换数据量的兑换率**：agent 十轮策展的 10k（33.7）≈random 100k（33.7）、Heavy II 34.9 超过 random 200k（34.0）——**搜索算力对数据量的兑换率约 10-20×（在本设置）**；50 iter 无平台=finite-data increasing-compute 的第三条路（除复用数据/合成扩数据外，**搜索怎么选数据**本身是算力去向）。
- **金子⑥ 污染审计协议细节（B.2）**：精确 SHA-256+8-gram ≥0.8 近重复、high_risk 强制改——agent 能读评测日志的环里，防「造贴近评测的数据白拿分」是基建位（连 #17 AI Scientist 绕资源限制事故——评测可读=越权面）。
- **金子⑦ 从零预训练实例的存在=轴判读修正**：DataComp-Small 实例是 **random-init CLIP 从零训练**——「八篇全 post-training 栈」需修正为：**Curation-Bench 含一个小尺度从零预训练实例**（12.8M seen、单 H100、~4h/run）——agentic 策展环在预训练方向已有一只脚；但 scale=DataComp-Small（小赛道），**离中训练尺度仍差两个量级以上，空位判断维持**。

## 5 库内连线
- **#100 RSI-Prime**：互补对——RSI-Prime 教模型「怎么做训练」（轨迹→教材），Curation-Bench 测 agent「能不能做数据研究」（策略搜索）；两文同周深读，L1 训练侧两把尺子。
- **#103 Resolution Hypothesis**：trajectory 四标签=把「分辨好坏」操作化的测量侧回应；金子③（有效率降但终分升）正是 #103「selection 比 generation 难」的具象。
- **#51 EvalResearchBench / #37 RSI Arena**：评测线三连——#51 评研究 agent、#37 评训练 agent、本文评策展 agent；#37 的长度 hack/分数-偏好脱钩教训对应本文金子⑥污染审计。
- **#105 AdaObs**：Light scaffold「扩 awareness 不改 execution」vs AdaObs「观测面自适应」——同属「研究支持系统该进什么信息」的问题。
- **#62 SimpleTES**：E.1 的「数据研究没有 oracle 告诉你策略是否科学」=评测稀疏 noisy 的同层问题；本文 Heavy I 降分读数对 #62 预算结构是反例侧注（结构化不必然增益）。
- **DataComp / ICONS / ARDS**：库外锚（数据隔离原则上游/被测人类基线）；RSIBench-Data（2607.25886）=同域姊妹基准（Gödel Forest 的尺子，watch 池）。
- **姊妹库 data-mixture-insights**：金子①②（八家族+配比家族天花板）+金子⑤（兑换率）+金子⑦（预训练实例修正）已追注接口文档。

## 6 诚实边界
主实例=VLM 指令微调（非 LM 预训练，自认「结论可能不迁移到 pretraining mixtures/code/math」）；**DataComp 数字只在图 3(b)、正文无精确值**；轨迹标签由 LLM 标 LLM（标注器判断引入，论文自认 rubric+示例披露缓解）；heavy scaffold 非全因子分解（论文自认是 design pattern 不是因果分离）；跨模型泛化仅 +0.4；v1 无 venue；agent 池=闭源为主（Claude Code/Codex）+两开源（OpenHands 挂 Kimi/Qwen）。

## 7 遗留（深读新设）
- DataComp 实例精确数字（从开源 repo/图 3b 复算）。
- 技能卡库的内容与覆盖率（多少篇论文→多少张卡）。
- 与 DataMaster（2605.10906，15a，同域更早）的分工关系——DataMaster watch 池待评。
- LM 预训练/中训练实例是否会出现在后续版本（升 T1 观察：社区采用+预训练实例）。

## 8 裁定
**T2 [高价值]（深读坐实）**。加分：数据策展环首尺+八家族分类学（对配比线是现成坐标系）+轨迹诊断方法论+污染审计协议+开源可复算+多机构学术线+DataComp 预训练实例（轴判读修正贡献）。不加 T1：v1 无 venue、主实例 VLM 微调域、DataComp 数字未在正文坐实、轨迹标注 LLM 依赖。**升级触发器**：venue（NeurIPS/ICLR 级）+社区采用（第三方在 LM 预训练域用此 harness）+DataComp 数字复算。
