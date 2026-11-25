# RSIBench-Infra（训推基建 RSI 的生产规模基准）深度分析

**总表条目**：#104｜arXiv 2611.24395（v1 2026-11-18）｜T2 **[高价值]**（轻评估⑦→**深读坐实**）｜深读批次⑳（2026-11-25，PDF 全文 22 页 + 附录；HTML 版不存在——PDF 路线 pymupdf 提取）

**【一句话】**#39 InfraRSI 线的**基准化**：20 个训推任务（16 推理+4 训练）、七引擎组（vLLM/SGLang/TensorRT-LLM/SkyRL/verl 等）、**1-32 加速器跨最多 4 节点、NVIDIA GPU+昇腾 NPU 双硬件**、模型 1.7B（Qwen3-TTS）到 2.8T（Kimi K3）——agent 从官方 recipe 起改 engine/kernel/指令级全栈，**OCI image-layer 提交协议**（单层捕获全部修改、每 rank 同 digest）+**独立 Benchmark Job 永不加载候选层**+LLM judge 审计轨迹防作弊；130 个正式评分中 **66.2% 达 >1.10× 验证加速**（GPT-6 Astra 领跑 16/20）。基线只吃掉分析性 SOL 的 **2%-36%**（多数服务任务）——headroom 巨大是 agent 能找到增益的底层原因。

## 1 出处与身份
- **8 人，工业-学术联合（坐实，机构待核已修正）**：Hanlin Wu/Yifei Wang（共同一作）/Zhicheng Cai/Huan-ang Gao/Wei-Ying Ma/Ya-Qin Zhang/Jingjing Liu/Hao Zhou（通讯 zhouhao@air.tsinghua.edu.cn）——**字节 Seed+清华智能产业研究院（AIR）+SIA-Lab（清华 AIR×字节 Seed 联合实验室）**。Huan-ang Gao「疑清华 HPC 系」待核字段清除（清华 AIR 坐实）。
- **释放**：项目页 rsibench-infra.github.io；v1 2026-11-18 无 venue；任务规格/质量门细节在附录 B.1。
- **定位**：自认「首个已知的生产规模、跨栈软件修改、任务特定约束下的 agent 端到端训推优化基准」。RSI 框架位：**更快的 rollout 生成+更快的模型更新=更短自改周期、同算力预算内更多迭代**——把「基建效率」正式定义为 RSI 环的组成部分。
- **撞名纪律（重要）**：论文自引 RSIBench-Data〔11〕与 RSI-Exam〔17〕为**不同工作**——本篇 RSIBench-Infra 2611.24395 ≠ 库内 #152 RSIBench-Data 2607.25886，引用必带 arXiv ID。
- **发现路径**：轻评估批次⑦→深读队列②→用户裁定「继续读」（批次⑳收官篇之一）。

## 2 基准构造（§3——可复现纪律是设计核心）
- **三段构造**：官方 cookbook（推理：vLLM/SGLang/TensorRT-LLM 模型专属部署食谱；训练：钉死版本的 verl recipe+Tinker 兼容参考实现，SkyRL-Tinker/verl-tinker 执行）→任务规格（task.yaml 绑硬件+launch.sh 启动契约+质量与负载）→**准入校准**（oracle 验证官方参考运行+质量门+性能基线）。「被准入的基线不假设是最优实现」——诚实起点声明。
- **OCI image-layer 提交协议（金子级设计）**：agent 的全部修改（engine 仓库/库/其他目录）打包成**单一 OCI 文件系统层** submit_oci.tar.gz；独立 Serve/Train Job 在干净基线上校验 digest 后逐 rank 应用同一层——**重建已知基线之上的提交状态、不继承 Agent Job 运行时状态**；**Benchmark Job 从不加载候选层**（工作负载生成/测量/评分与 agent 修改隔离=反 benchmark hacking 的结构保障）；LLM judge 额外审轨迹与提交变更找捷径/违约。训练任务里可信评估代码与参考资产留在候选环境之外、候选修改之上先装钉死支持文件。
- **质量门（每任务独立）**：语言=GSM8K/LongBench v2、视觉语言=MMMU、视频/世界模型/动作生成=参考输出一致性、语音=中文 ASR WER；**训练=frozen-input replay 逐步 loss+全局梯度范数曲线对照参考执行**（VL GRPO 加检 policy/KL loss+actor/参考前向输出+**独立 KL 梯度探针**）。
- **性能基线（生产代表性）**：如 Kimi K3 任务=256k 上下文、4 节点×8 GPU、**从 Terminal-Bench 2.1 轨迹构造 894 请求×20 会话的 agentic trace**（Mooncake 格式回放、保留输入输出长度与会话前缀复用模式）、固定 1 小时窗口 concurrency=1 测输出吞吐+TTFT/TPOT 诊断。启动/加载/质量评估**不计入性能分母**。
- **评分**：验证加速 s_i=提交吞吐/固定基线吞吐；**Faster 门槛严格 >1.10×（明确为降噪声敏感度）**；Median Speedup 抗离群；**正式零分（部署失败/质量门不过/合规拒绝）计入聚合、从不剔除**；有效减速保留 <1 比值。

## 3 主结果核验（Table 2 逐行）
- **七配置（8 小时预算/reasoning=max/1M 上下文；Astra 用 Codex、Claude 用 Claude Code、其余用 OpenCode）**：GPT-6 Astra **16/20（80%）**、中位 **1.3335×**、mean SOL 0.667、平均 3:36（预算的 45%）；Claude Opus 5.5 8/10（80%）/1.3431×/0.711†（**只完成 10 任务——其余 10 个触发 API 安全策略错误**：逐模型部署竞品模型时被安全策略终止，被拒任务名单完整给出）；GLM-5.3 与 Qwen3.8-Max 各 14/20（70%）；DeepSeek V4 Flash 与 Kimi K3 各 12/20（60%）；Seed Evolving 10/20（50%）。
- **总计**：130 个正式评分中 86 个（66.2%）>1.10×。
- **优化行为模式（130 提交多标签，立标签纪律：改动须在最终 OCI 工件中**存在且启用**，「文件或关键词存在不足以立标签」；回滚实验/禁用代码/预装补丁排除）**：graph/compilation 31 Faster/35（**88.6%**）、cache/memory 31/39（79.5%）、**speculative decoding/MTP 22/23（95.7%）**、specialized GEMM/GEMV **15/15（100%）**、operator fusion 15/20、attention backend 16/17（94.1%）。不同模型偏好不同：Astra/Claude 重算子融合（各 6 任务）、GLM/Qwen 重缓存/内存（行为模式总数 62/61 但耗时长——**Astra/Claude 用更少的尝试拿同档 Faster 率**）。
- **具体案例（跨层）**：Astra GLM-5.3 提交=small-head BF16 稀疏 MLA kernel→1.7958×；Qwen Inkling=原生 MTP+更大 GPU KV 池+分层主机缓存→**3.3276×**；Claude LLaDA=block-causal prefill+向量化解码+状态复用→**6.1062×**（全场最大）。
- **失败解剖（44 非 Faster=35 个 ≤1.10× 正分+9 个正式零）**：零分=4 部署失败+4 质量门不过+1 合规拒绝。**Qwen SFT 原始加速 1.2540× 但质量门不过记零**；Seed TTS 过语音质量门但**因改 repetition penalty 被合规拒绝**——性能/质量/合规三轴分离评估的理由展示。
- **成本-效率**：Pareto 前沿因指标而异（Faster Rate 前沿=DeepSeek/GLM/Astra；Median 前沿=DeepSeek+Astra；SOL 前沿含 Kimi——0.62667 微超 GLM 0.62639）；价格表=OpenRouter 2026-11-13 快照。

## 4 深读金子（摘要外）
- **金子① OCI 层=可复现纪律的容器原生形态**：git patch 在「agent 可改栈内任何东西」的设定下不够（patch 不覆盖安装目录/编译产物/环境状态）；**单一 OCI 层+逐 rank 同 digest+独立 Benchmark Job 不加载候选层**——三层结构把「提交了什么」「在哪跑的」「谁评分」完全解耦。连 #41 reef 的 SHA 钉版纪律：这是同一纪律在容器层的天然形态，且**评估侧隔离是结构性的（不是承诺性的）**。
- **金子② 训练任务的确定性重放质量门**：frozen-input replay 对照逐步 loss+全局梯度范数曲线（+KL 梯度探针）——**训练侧也有「check↔official」问题，其解法=参考曲线重放**。对姊妹库：中训练配方实验的「不伤基线」检查可用同款（冻输入重放对照 loss/梯度曲线），比端到端评测便宜得多。
- **金子③ 分析性 SOL 参考+基线占比表（b/s）**：AISimulate 从硬件规格（算力/带宽/精度）估算每任务分析上限，**基线只达 SOL 的 1.97%（GLM-5.2@32×910B2）到 70%（LingBot）**，多数服务任务 2-36%——**headroom 的绝对量尺**：agent 的 1.1-6× 加速在 SOL 视角下只是把 b/s 从 0.02 推到 0.1 量级，远未见顶。「便宜尺子」家族的基建版：不跑模型就能定改进空间的上下界。
- **金子④ 单 H20 五任务 Lite Set**：降低复现门槛的 Accessibility 设计（文本/VL/扩散语言/视频四类负载+对应引擎）——基准开源生态的「梯子」思想：先在单卡跑通，再上全量。
- **金子⑤ Claude API 安全策略=10 任务被拒的失败模式**：Claude Code 在被要求优化竞品模型（DeepSeek/GLM/Kimi/Nemotron/Qwen）的部署时触发 400 安全策略错误——**agent 可用性的非技术边界首次以任务清单形式坐实**；聚合口径诚实（10 正式任务决定全部聚合、缺项不转零）。
- **金子⑥ 「one run per pair、排名应读作点估计」诚实声明**：不提供重复运行置信区间、不做预算受控比较——#103 方差纪律的主动自我声明；**1.10× Faster 门槛=明写的降噪设计**（同一纪律的预防性形态）。另有「不选多次尝试最佳分」承诺。
- **金子⑦ 行为模式的「证据要求」表（Table 8）**：19 个优化类别逐一定义立标签所需证据+排除项+保留案例（如 graph/compilation：「profiling trace 不是编译证据」；operator fusion：「import 已在用的融合算子不算」）——**标签注水的制度化防线**，任何做行为归因的研究可搬。

## 5 库内连线
- **#39 InfraRSI（华为诺亚）**：本篇=该线的基准化收官；#39 遗留「基线自家复刻待第三方复核」——本篇用**官方 recipe 做基线+准入校准**绕开自家复刻问题（不同解法，同题）；四负载（#39）vs 20 任务+双硬件（本篇）——规模与多样性大幅扩容。
- **#41 reef**：金子①（OCI 层=容器原生 SHA 纪律）；meta_harness RESULTS.md 的 30×2 重评打平也照记——本篇 1.10× 门槛+点估计声明是同一诚实传统。
- **#62 SLDBench**：结构化评测预算家族（#62 定预算结构/本篇定证据结构）。
- **#103**：金子⑥（点估计声明+1.10× 降噪门槛）；LLM judge 在本篇只做**作弊审计**（不是选择器）——与 #103「judge 当选择器失败」互补：judge 审轨迹找违约有用、judge 选工件无用（两种用法的分界读数）。
- **#152 RSIBench-Data**：撞名纪律（2607.25886 vs 2611.24395）；论文互认互补（数据研究 vs 基建研究，固定对方为环境）。
- **#104 昇腾口径**：国产加速器（Ascend 910B2）首次以**任务级双硬件**进基准口径（此前库内只有 GPU）——5 个 NPU 任务（GLM-5.2 32×NPU 是全场最大部署）。
- **姊妹库 data-mixture-insights**：金子②（冻输入重放=训练侧便宜检查）+金子③（SOL 量尺思路=配比实验的 headroom 先验）；训推效率直接改中训练梯子的成本结构（同预算更多步数）。

## 6 诚实边界
单 run 点估计（自认）；8 小时预算未做受控比较；Claude 覆盖不完整（10/20，API 策略）；行为归因为语义审查非双人独立标注（自认）；「official recipe 基线」绑定特定引擎版本（基线会随版本漂移）；无 venue、v1；项目页在但代码/任务包释放状态未核。

## 7 遗留（深读新设）
- 重复运行方差补充（#103 式 τ/σ 测量在这套任务上的读数——训练任务的 run-to-run 方差尤其未测）。
- Claude 安全策略拒答的细粒度归因（模型身份 vs 部署内容 vs 工具链）。
- 任务包/基线镜像公开后第三方复算；leaderboard 开放性（触发器原项）。
- 昇腾任务的社区可复现性（910B2 环境获取门槛）。

## 8 裁定
**T2 [高价值]（深读坐实）**。加分：基准空缺填补（生产规模+跨栈+双硬件+训推双任务）+OCI 协议/独立评估/SOL 参考/证据要求表四件可搬的设计+66.2% 主结果+行为模式统计+失败三轴分离+诚实边界主动声明。不加 T1：单 run 点估计、无重复方差、Claude 覆盖缺口、无 venue、材料释放未核。**升级触发器**：任务包公开+第三方复算；重复运行方差补测；venue 落地；被其他组用作标准尺。
