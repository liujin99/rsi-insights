# EvalResearchBench（ERB）深度分析

> 条目：#51 ｜ arXiv 2610.04184（v1 2026-10-03《Can AI Agents Design Their Own Evaluations?》→ v2 2026-10-16 改名《Can AI Agents Build Their Own Feedback for Recursive Self-Improvement?》）｜ OSU+UCSD+UCSC+PSU+AG2 AI ｜ 深读日期：2026-11-17
>
> **一句话**：第一个把"agent 能不能自己设计评测（自建反馈仪器）"标准化的 benchmark。答案：**排序大体可复制（PA≈75%），分数水平复制很差（CCC≈23/100），可见参照上拟合最好 ≠ 迁移最好**——"自建反馈"当前是部分能力。

## 1. 问题定义：autonomous evaluation research

- **动机**：RSI 闭环里最贵的一环是评测——反复跑复杂 benchmark 又贵又慢；人类专家靠选子集/设计紧凑套件降成本（tinyBenchmarks 线）。ERB 问：这步能否让 agent 自己做？
- **缺口定位（论文自己给的谱系，可信）**：PostTrainBench / RSIBench-Data / AlphaEvolve 都是"自动改东西、评测人类给定"；AutoBencher / BenchAgents / BenchBench 是"自动出题"但非完整研究问题。ERB 第一个提出**完整问题**：agent 自选测量策略 + 自花预算 + pilot 迭代 + 交付能追踪复杂目标 benchmark 的可执行评测器。
- **任务设计**：researcher（Codex / Claude Code harness 里的 LLM agent）拿到目标描述、可见参照分、候选 API、计量预算（**3 小时 + $100 推理 + $100 开发测试**，独立评估另计 $120 总额/$30 每候选）；选/合成任务、写 grader、pilot 试错后**冻结**可执行评测器（合同：≥100 道不同题目 + 可执行 grader + 固定聚合权重）。
- **独立评估**：冻结后在全部候选上执行，对照 14 个目标 benchmark。**密封纪律**：9 个候选 + 8 个密封目标对 researcher 不可见——候选可见性与目标可见性分开控制。
- **度量**：Score = 100×Lin 一致性相关系数（CCC，水平+离散+排序全对齐才高分）；PA = 候选对排序与目标一致的比例。
- **方法学干净点**：冻结 + 密封 + 独立评估三件套——评估的"设计"与"执行/裁决"分离，恰是本库准则6 纪律的基准化实现。

## 2. 实验设置与主结果

**设置**：9 个 researcher（GPT-6 Astra、GPT-5.6 Sol、GLM-5.3、DeepSeek V4 Pro、Kimi K3、Fable 5.1、Opus 5、Grok 4.7、Fable 5；GPT 系/Grok 走 Codex，其余 Claude Code）× 13 个候选（4 可见：GPT-5.5/GLM-5.2/DeepSeek V4 Flash/Qwen3.8 Max；9 密封：GPT-6 Astra/GPT-5.6 Sol/Kimi K3/MiMo V2.5 Pro/DeepSeek V3.2/Gemma 4 31B/Qwen3.5 397B-A17B/GPT-OSS 120B/Nemotron 3 Ultra）× 14 个目标（6 可见 + 8 密封；编码 6 / 协作 5 / 推理 3，含 DeepSWE、Terminal-Bench 2.1、GDPval、BrowseComp、HLE、Harvey LAB-AA 等）。基线 **Random120**：人类定源的分层随机采样（16 源配额 40×3 域）。

**主结果（Table 2 总体列）**：

| researcher | Score | PA | 备注 |
|---|---|---|---|
| GPT-6 Astra | **22.79** | 69.54 | 总体 Score 第一（协作/推理强） |
| Grok 4.7 | 22.59 | **74.87** | 总体 PA 第一 + **执行成本最低** |
| Kimi K3 | 18.03 | 74.58 | 密封协作 PA 87.07 单点最高 |
| Fable 5.1 | 21.96 | 67.80 | **dev Score 第一但 sealed 不是** |
| GLM-5.3 | 14.27 | 64.72 | |
| Opus 5 | 21.79 | 64.88 | dev 推理 77.99 单点最高 |
| DeepSeek V4 Pro | 10.49 | 63.50 | 迭代多但不及基线（教训样本） |
| Fable 5 | （负） | （负） | **评估预算烧穿**，插补情形 |
| Random120（人类） | 13.10 | 68.29 | 强基线 |

## 3. 六个可引用结论（证据强度：受控实验，方向性结论层）

1. **排序可复制、水平难复制**：最优 PA≈75%，最优 CCC 仅 ~23/100——复现"谁比谁强"远易于复现"强多少"。给 RSI 闭环的启示：agent 自建的便宜反馈仪器**可用于探索方向的粗排序**，用于精细分数对标则不够格。
2. **天花板 91% = 目标间互不一致**：14 个目标 benchmark 彼此只 91% 一致——"质量标尺无共识"在**仪器层**就已成立（连 #37 在 L1 层的"benchmark↔人评脱钩"结论：两层证据互相印证）。
3. **dev 最优 ≠ sealed 最优（准则6 定量实锤）**：Fable 5.1 可见参照上拟合最好、迁移后让位 Grok——"对可见参照过拟合不迁移"第一次被受控量化。自改进系统里"在能看见的评测上刷到最好"不等于反馈仪器好。
4. **人类随机采样是强基线**：8 个 agent 评测器中 7 个 Score 胜 Random120、仅 4 个 PA 胜——"自动设计评测"尚未显著优于"人类设计个便宜采样"；DeepSeek V4 Pro 案例显示反复迭代本身不产生信息量（挑出能拉开候选差距的任务才产生）。
5. **低成本与高质量可共存**：Grok 执行成本最低且 PA 最高——pilot 反馈把答案截断问题转成"短题+本地 grader"设计。对 Q1（反馈信号成本）是正信号：**便宜反馈仪器这条路通**。
6. **三个具体失败模式**：答案截断、评估预算耗尽（Fable 5 烧穿即弃赛）、少数题权重垄断（Grok 协作域 6/20 题占几乎全部权重——等权重重算 Score 大涨、PA 不动：冻结套件里有测量信号但被自选权重浪费）。失败模式清单本身就是下一版 researcher 的改进清单。

## 4. 与本库的接口

- **Q6（AI 产出研究质量的度量）**：本库判断"没有度量就没有飞轮（L4 缺评估环）"——ERB 给了这个缺口**第一把尺子**，当前读数=部分能力（见结论 1/3/4）。
- **准则6（生成者=评估者）**：ERB 的冻结+密封+独立评估=该纪律的基准化范本；其 dev→sealed 不迁移结论=本准则首次受控定量证据（此前只有机理推断与 #37 的间接证据）。
- **Q1/Q2（信号可靠性与成本）**：结论 5（便宜+高一致可共存）是"agent 自建廉价 proxy"路线的正信号；结论 4（人类基线难胜）是冷却剂。
- **L5 测量线定位**：与 #37 RSI Arena 互补——Arena 测"人评当外部验证器"（人做尺子），ERB 测"agent 自建尺子"；两者都以"密封/盲"为核心设计。L5 现有三件套：METR（进度标尺）+ 人竞场（信号标尺）+ ERB（自建反馈标尺）。
- **连 #05 AlphaEvolve**：其"用户提供评测函数"的边界被 ERB 正面测量——评测函数自动化这条腿当前只能半自动。

## 5. 裁定与触发器

**T2 [高价值]**：知名学术组（Qingyun Wu=AutoGen/AG2 创始人系、Huazheng Wang〔OSU〕、Jishen Zhao〔UCSC〕）+ 方法学干净（冻结/密封/独立评估）+ 结论直接可用（Q6/准则6 首批定量证据）+9 个前沿模型当 researcher 的覆盖面。减分：无 venue（v2 preprint）、社区采用低（repo 5⭐、首版单点）、绝对分数低说明任务远未解决（这本身是领域读数）。

**升级触发器**：venue 接收 / 第二届（换 researcher 面板验证结论稳定性）/ 第三方采用 ERB 做自改进系统筛选 / 目标面板扩展（当前 14 目标互不一致度 91% 的面板构成敏感性待复验）。

**遗留待核**：repo 内容完整度（代码+数据是否足以复算 Table 2，5⭐ 未验）；v1→v2 除改名外的正文差异未逐节 diff；GLM-5.3 既是 researcher 又有 GLM 系候选的自评交叉风险（密封设计下应无泄漏，但表 1 构成值得复核）。
