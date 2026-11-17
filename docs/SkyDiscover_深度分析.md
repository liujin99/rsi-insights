# SkyDiscover 深度分析

**总表条目**：#61｜ACM CAIS 2026（Proceedings of the ACM Conference on AI and Agentic Systems，Industrial Spotlight，pp. 1223-1227，DOI 10.1145/3786335.3813221，CC-BY 混合 OA）｜T2（维持）｜深读批次⑤（2026-11-17，官方 BibTeX+S2 摘要+姊妹博客+repo README 交叉）

**【一句话】**Berkeley 把 LLM 驱动的"发现"当**基建层**做——四组件分解（Context Builder/Solution Generator/Evaluator/Solution Selector）+可编程控制层，把 OpenEvolve/GEPA/ShinkaEvolve 的紧耦合单体拆成可插拔件；两个自适应算法（AdaEvolve/EvoX）在 200+ 任务上给出开源最强、6/8 数学与 6/6 系统匹超 AlphaEvolve 与人类 SOTA。

## 1 出处与身份
- 18 作者：Shu Liu*/Mert Cemri*/Shubham Agarwal*（共三共一）/Krentsel/Naren/Mang/Li/Gupta/Maheswaran/Cheng/Pan/Boneh/Ramchandran/Sen/Zaharia/Dimakis/Stoica——UC Berkeley（SkyComputing 命名系谱：SkyPilot/SkyComputing/SkyDiscover）+Stanford（Ethan Boneh）+Bespoke Labs；**一作 Shu Liu=#60 EvoX 共一**（同一批人两文）。
- **CAIS'26=ACM Conference on AI and Agentic Systems（新会议，本库首个 CAIS 条目）**；Industrial Spotlight 类别=产业系统论文赛道；30 引用（S2）；官方 BibTeX 在 repo README。
- 深读材料说明：ACM 页 Cloudflare 拦直抓、无 arXiv 版——用官方 BibTeX+S2 摘要+**姊妹博客**（2026-03-03，结构同论文：Background/Challenge/Framework/Results/Case Studies）+README 交叉重建，五个来源互洽；5 页短文信息密度高，无大段缺失。

## 2 问题：紧耦合单体难扩展、难比较
- 现有系统共享同一高层循环（population→经验注入 prompt→生成→评估→回写）但实现紧耦合：OpenEvolve=MAP-Elites 选择+固定 prompt 模板；GEPA=Pareto 前沿选择与重组/变异算子耦合——**改一个组件要重写核心基建**。双目标：Generality（统一评估）+Flexibility（新方法不动单体）。

## 3 框架：四组件 + 可编程控制层
- **Context Builder**（组装问题上下文+历史解知识〔反思/反馈〕）/ **Solution Generator**（LLM+可选工具产候选）/ **Evaluator**（打分并把反馈写回解库）/ **Solution Selector**（维护解池并选谁指导下轮：random/Top-K/MAP-Elites/Pareto）。
- **控制逻辑本身可编程**——四组件的编排暴露为接口，"AI 适配甚至优化优化过程本身"成为一级公民（=元层接口化的正面表述，连 #60）。
- ~2.5K LoC 实现两个 SOTA 算法——模块化红利直接量化（连 #39 InfraRSI 的"薄基建"论：基建越薄，算法迭代越快）。

## 4 两算法（与 #60 分工划定）
- **AdaEvolve**（arXiv 2602.20133，Cemri/Agarwal 领衔，**未入库=sweep 候选**）："Progress-Aware Guidance"——把发现视为**非平稳优化**（类比 AdaGrad/Adam，但零阶），三层自适应：① Global（多岛 subpopulation+**bandit 调度**算力分配/动态建岛）② Local（岛内探索-利用平衡）③ Meta-Guidance（停滞时注入新解技术）。
- **EvoX**（=#60，已深读 [高价值]）：策略元进化——"The What/The How"双耦合环，停滞触发 LLM 重写搜索代码。
- **分工与互引结论**：EvoX 论文**通篇不提 SkyDiscover**（rg 零命中）——互引单向（框架文引算法文，算法文独立成篇）；repo README 为生态枢纽（Optimize/Synthesize 两线+AdaEvolve/EvoX 双算法+行业采用"including Google and Uber"）。**生态=三文一仓**：框架文（CAIS）+AdaEvolve+EvoX。

## 5 结果（固定预算 100 代+固定模型 GPT-5/Gemini-3.0-Pro 下）
- **数学优化 8 任务**：全部 LLM 框架中最优；对 AlphaEvolve **6/8 匹或超、3 个反超**（圆填充/圆填充矩形/3D min-max 距离）；矩形圆填充 EvoX 2.36583237 vs 2.36583213（与 #60 深读数字一致——两文同一实验）。
- **系统优化 6/6 超人类 SOTA**：跨云迁移成本 −41%、MoE serving GPU 负载均衡 +14%、GPU 模型放置 KV-cache 压力 −29%（=生产可落地发现，Google/Uber 采用的载体）。
- **Frontier-CS 172 题**：开源最强，mean 62.6/median 75.5；vs OpenEvolve +24%/+34%、vs GEPA +22%/+34%、vs ShinkaEvolve +30%/+63%。
- 案例研究展示**被发现的程序本身**：圆填充 21 从"全零空程序"出发，发现的解=六边形/手动构型种子+批处理物理松弛（刚度退火）+SLSQP 精修管线——发现物是**可读的算法管线**而非黑盒权重。

## 6 库内连线
- **基建化判断升级确认**：sweep 时的"基建化已从 harness 层蔓延到发现层"（连 #41 reef/#45 RSIHub）现在有**正式 venue+工业采用**背书——发现层的 Sky 命名系谱=harness 层的基础设施化在 L3 重演。
- **开源 vs 闭源**：开源框架+开源算法（GPT-5/Gemini 为租用骨干）匹超闭源 AlphaEvolve——**闭源优势在策略层被追平、剩骨干与算力层**（连 #62 GPT-OSS-120B 单模型 28 SOTA 的同型结论）。
- **元层接口化**：控制层可编程=#60 元进化得以实现的前提——先有可编程接口，才有策略元进化（三层"学怎么搜"里 #60 的基建条件首次显式化）。
- Q1：2.5K LoC 双 SOTA=基建薄度与算法产出率的第一个粗比率（连 #39）。

## 7 裁定与遗留
- **裁定**：T2 维持——5 页短文、算法实质在 #60/AdaEvolve 两文；本篇价值=基建化宣言+venue+采用证据+四组件分解的命名权（后续论文将沿用其术语，引用锚点价值高）。不升 [高价值]：无独立新读数。
- 遗留：① AdaEvolve（2602.20133）入库评估——bandit 岛群调度=元级第三种信号（连 #38/#60 谱系）；② SkySynth（SkyDiscover-Synthesize，2026-09 博客：KV store 2.3× 超 Redis/FASTER、推理引擎 2.2× 超 vLLM/SGLang、correct by proof〔2605.23109〕/by test〔2605.24096〕两文）=**合成线**，harness/发现双库都沾边，作 sweep 候选；③ CAIS'26 新会议的定位与录取率待观察（本库引用格式沿用官方 BibTeX）；④ "used across industry including Google and Uber"为 repo 自述，采用证据待独立核实。
