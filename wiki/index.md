# Wiki Index

本知识库是关于大语言模型（LLM）提示工程的综合知识库，整合自 Google、Anthropic、OpenAI 等机构的官方文档和最佳实践。

---

## Sources（来源摘要）

### 提示工程资料
- [[摘要-gemini-api-prompting-strategies]] — Google Gemini API 提示设计策略中文指南
- [[摘要-prompt-engineering-2025-guide-promptbuilder]] — Prompt Engineering 2025 完整指南（PromptBuilder）
- [[摘要-ai-prompt-engineering-2025-2026-espo]] — AI 提示工程完整指南 2025-2026（Espo.ai）
- [[摘要-complete-prompt-engineering-guide-2025]] — 完整提示工程指南 2025（BrilliantPrompts）
- [[摘要-anthropic-prompting-best-practices]] — Anthropic Claude 提示最佳实践（官方）
- [[摘要-google-prompt-engineering-whitepaper]] — Google Prompt Engineering 白皮书（官方）
- [[摘要-5c-prompt-contracts-paper]] — 5C Prompt Contracts 研究论文

### CANN昇腾社区资料
- [[摘要-算子与高阶API共享临时Buffer]] — 算子与高阶API共享临时Buffer提升内存效率
- [[摘要-纯搬运类算子VECIN和VECOUT建议复用]] — TQueBind接口优化纯搬运类算子
- [[摘要-通过Unified-Buffer融合实现连续vector计算]] — UB Buffer融合减少GM搬入搬出
- [[摘要-Vector算子灵活运用Counter模式]] — Counter模式简化矢量计算减少Scalar开销
- [[摘要-选择低延迟指令-优化归约操作性能]] — BlockReduceSum+WholeReduceSum组合最优
- [[摘要-通过BT-Buffer实现高效的bias计算]] — BT Buffer存放bias实现一次Mmad完成矩阵乘加bias
- [[摘要-通过FP-Buffer存放量化参数实现高效随路量化]] — FP Buffer存放量化参数实现随路量化
- [[摘要-通过L0C-Buffer数据暂存实现高效的矩阵乘结果累加]] — L0C暂存矩阵乘结果实现累加
- [[摘要-Matmul使能AtomicAdd选项]] — Matmul使能AtomicAdd避免额外UB分配
- [[摘要-较小矩阵长驻L1-Buffer-仅分次搬运较大矩阵]] — 小矩阵长驻L1减少搬运次数
- [[摘要-L2-Cache切分]] — L2 Cache切分策略提升命中率

---

## Entities（实体）

### 公司/组织
- [[Google]] — 全球领先的科技公司，Gemini 模型开发者
- [[Anthropic]] — AI 安全研究公司，Claude 模型开发者

### 模型/产品
- [[Gemini]] — Google 的大型语言模型家族
- [[Claude]] — Anthropic 的大型语言模型家族

### CANN昇腾生态
- [[昇腾]] — 华为AI处理器品牌
- [[CANN]] — 华为昇腾计算架构（Compute Architecture for Neural Networks）
- [[Ascend C]] — CANN下的C/C++算子开发API
- [[AI Core]] — 昇腾处理器的核心计算单元

---

## Concepts（概念）

### 核心概念
- [[Prompt_Engineering]] — 提示工程：设计与优化 LLM 输入的技术学科
- [[Context_Engineering]] — 上下文工程：从单个提示到系统上下文管理的范式转变

### Ascend C性能优化
- [[DoubleBuffer]] — 基于MTE与Vector队列并行性的数据搬运与计算并行优化
- [[Tiling策略]] — 将大数据块切分为小数据块以适应硬件限制
- [[L2_Cache切分]] — 使能L2 Cache切分策略提升命中率
- [[核间负载均衡]] — 调整切分策略使尾核均匀分布
- [[头尾开销优化]] — 降低算子头尾开销的优化技术
- [[Unified_Buffer融合]] — 前一次计算输出直接作为下一次计算输入
- [[AtomicAdd]] — Matmul结果矩阵C累加到GM地址的矩阵D
- [[Counter模式]] — Vector算子的mask模式简化
- [[Bias_Table_Buffer]] — BT Buffer存放bias实现一次Mmad完成矩阵乘加bias
- [[Fixpipe_Buffer]] — FP Buffer存放量化参数实现随路量化
- [[L0C_Buffer]] — L0C暂存矩阵乘结果通过cmatrixInitVal累加
- [[归约操作]] — BlockReduceSum + WholeReduceSum组合最优
- [[同地址访问冲突]] — 512字节对齐导致的多核访问串行化规避
- [[DataCopyParams]] — 搬运API参数配置实现连续搬运或固定间隔搬运
- [[TQueBind]] — VECIN与VECOUT绑定省略拷贝步骤
- [[Scalar常量折叠]] — 编译器优化技术，TPipe外置可触发
- [[Workspace]] — 算子运行时的工作空间

### 提示技术
- [[Zero_Shot_Prompting]] — 零样本提示：无示例直接提问
- [[Few_Shot_Prompting]] — 少样本提示：通过示例引导模型
- [[Chain_of_Thought]] — 思维链：要求模型展示推理步骤

### 提示框架
- [[5C_Framework]] — 5C Prompt Contract：极简、token高效的提示设计框架
- [[APE_Framework]] — APE 框架：Action + Purpose + Expectation
- [[CO-STAR_Framework]] — CO-STAR 框架：内容创作专用框架
- [[RISEN_Framework]] — RISEN 框架：复杂多步骤任务框架
- [[CRAFT_Framework]] — CRAFT 框架：Context + Role + Action + Format + Tone
- [[POWER_Framework]] — POWER 框架：Purpose + Output + Working + Examples + Refinement

### 高级技术
- [[ReAct]] — ReAct：推理与行动结合的代理范式
- [[Tree_of_Thoughts]] — 思维树：同时探索多条推理路径
- [[RAG]] — 检索增强生成：结合外部知识库

---

## Syntheses（综合分析）

- [[5c-prompt-markdown-note-taking]] — 基于 5C Framework 的 Markdown 知识笔记撰写提示词模板

（待创建：框架对比分析、模型特性对比等）

---

*最后更新：2026-04-17*
