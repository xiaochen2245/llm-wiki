# Wiki 操作日志

---

## [2026-04-12] ingest | 批量摄入提示工程核心资料

### 处理文件清单

**Articles（5 篇）：**
- `raw/01-articles/提示设计策略  _  Gemini API.md` — Google Gemini 中文指南
- `raw/01-articles/Prompt Engineering in 2025_ Complete Guide for ChatGPT, Claude, and Gemini.md` — PromptBuilder 指南
- `raw/01-articles/The Complete Guide to AI Prompt Engineering in 2025-2026.md` — Espo.ai 指南
- `raw/01-articles/The Complete Prompt Engineering Guide (2025).md` — BrilliantPrompts 指南
- `raw/01-articles/Prompting best practices-Anthropic.md` — Anthropic 官方最佳实践

**Papers（2 篇）：**
- `raw/02-papers/Goolge-Prompt-Engineering-whitepaper.pdf` — Google 官方白皮书（65 页）
- `raw/02-papers/5C Prompt Contracts .pdf` — 5C 提示契约研究论文

### 创建的来源摘要（7 个）

| 文件 | 描述 |
|------|------|
| [[摘要-gemini-api-prompting-strategies]] | Gemini API 提示设计策略 |
| [[摘要-prompt-engineering-2025-guide-promptbuilder]] | Prompt Engineering 2025 完整指南 |
| [[摘要-ai-prompt-engineering-2025-2026-espo]] | AI 提示工程 2025-2026 指南 |
| [[摘要-complete-prompt-engineering-guide-2025]] | 完整提示工程指南 2025 |
| [[摘要-anthropic-prompting-best-practices]] | Anthropic 提示最佳实践 |
| [[摘要-google-prompt-engineering-whitepaper]] | Google 提示工程白皮书 |
| [[摘要-5c-prompt-contracts-paper]] | 5C Prompt Contracts 论文 |

### 创建的概念页面（5 个）

| 页面 | 类型 | 核心内容 |
|------|------|----------|
| [[Prompt_Engineering]] | 核心概念 | 提示工程总览、七大要素、技术分类 |
| [[5C_Framework]] | 框架 | 5C 提示契约框架详解 |
| [[Chain_of_Thought]] | 技术 | 思维链技术、Zero-shot/Few-shot CoT |
| [[Few_Shot_Prompting]] | 技术 | 少样本提示最佳实践 |
| [[Context_Engineering]] | 范式 | 从提示工程到上下文工程的转变 |

### 创建的实体页面（4 个）

| 页面 | 描述 |
|------|------|
| [[Google]] | Google 公司及其 AI 产品 |
| [[Anthropic]] | Anthropic 公司及其安全研究 |
| [[Gemini]] | Google Gemini 模型家族特性 |
| [[Claude]] | Anthropic Claude 模型家族特性 |

### 冲突与发现

**知识冲突（已标注）：**
- 在 [[5C_Framework]] 页面中记录了「5C vs 复杂 DSL 之争」的知识冲突

**重要发现：**
1. **提示长度悖论**：研究表明 150-300 词是最佳长度，超过 3,000 tokens 性能显著下降
2. **CoT 悖论**：现代推理模型（GPT-5, Claude 4, Gemini 3）内部自动推理，显式 CoT 反而可能有害
3. **5C 效率优势**：平均仅需 54 tokens 输入，比 DSL 节省 6 倍以上
4. **Gemini 简化趋势**：Gemini 3 不再需要复杂提示工程，建议使用简化提示
5. **Context Engineering 范式**：行业正在从单个提示优化转向系统上下文管理

### 更新文件
- [[index.md]] — 重新组织了总目录结构
- [[log.md]] — 记录本次操作（本条目）

### 归档操作
所有 7 个源文件已移动至 `raw/09-archive/` 目录。

---

## [2026-04-12] query | 基于 5C Framework 设计 Markdown 笔记提示词

- **查询**: 根据 5C Framework 设计撰写 Markdown 格式知识笔记的提示词
- **引用**: [[5C_Framework]]
- **输出**: 已创建 synthesis 文件 [[5c-prompt-markdown-note-taking]]
- **更新**: [[index.md]] 已注册新 synthesis

### 提示词特色
- 遵循 5C 框架：Character/Cause/Constraint/Contingency/Calibration
- Token 高效（约 150 tokens）
- 包含完整的使用示例和变体建议
- 内置质量自检机制（Calibration）

---

## [2026-04-17] ingest | 批量摄入CANN昇腾社区性能优化文档

### 处理文件清单（11篇）

- `raw/01-articles/算子与高阶API共享临时Buffer-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/纯搬运类算子VECIN和VECOUT建议复用-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/通过Unified Buffer融合实现连续vector计算-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/Vector算子灵活运用Counter模式-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/选择低延迟指令，优化归约操作性能-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/通过BT Buffer实现高效的bias计算-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/通过FP Buffer存放量化参数实现高效随路量化-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/通过L0C Buffer数据暂存实现高效的矩阵乘结果累加-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/Matmul使能AtomicAdd选项-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/较小矩阵长驻L1 Buffer，仅分次搬运较大矩阵-CANN社区版9.0.0-beta.2-昇腾社区.md`
- `raw/01-articles/L2 Cache切分-CANN社区版9.0.0-beta.2-昇腾社区.md`

### 创建的来源摘要（11个）

| 文件 | 核心优化策略 |
|------|-------------|
| [[摘要-算子与高阶API共享临时Buffer]] | 共享Buffer空间提升单次搬运数据量 |
| [[摘要-纯搬运类算子VECIN和VECOUT建议复用]] | TQueBind接口省略VECIN到VECOUT拷贝 |
| [[摘要-通过Unified-Buffer融合实现连续vector计算]] | 前一次计算输出直接作为下次输入 |
| [[摘要-Vector算子灵活运用Counter模式]] | Counter模式减少主尾块计算和Scalar计算 |
| [[摘要-选择低延迟指令-优化归约操作性能]] | BlockReduceSum+WholeReduceSum组合最优 |
| [[摘要-通过BT-Buffer实现高效的bias计算]] | bias存BT Buffer一次Mmad完成矩阵乘加bias |
| [[摘要-通过FP-Buffer存放量化参数实现高效随路量化]] | 量化参数存FP Buffer实现随路量化 |
| [[摘要-通过L0C-Buffer数据暂存实现高效的矩阵乘结果累加]] | L0C暂存矩阵乘结果通过cmatrixInitVal累加 |
| [[摘要-Matmul使能AtomicAdd选项]] | AtomicAdd选项避免额外UB分配和搬运 |
| [[摘要-较小矩阵长驻L1-Buffer-仅分次搬运较大矩阵]] | 小矩阵长驻L1只分次搬运大矩阵 |
| [[摘要-L2-Cache切分]] | 使能L2 Cache切分策略提升命中率 |

### 更新文件
- [[index.md]] — 新增CANN昇腾社区资料分类（11个摘要）
- [[log.md]] — 记录本次操作（本条目）

---

## [2026-04-12] lint | 知识库健康检查

### ✅ 绿灯项
- 所有来源页面均有双向链接引用
- 所有概念页面均有双向链接引用
- 所有实体页面均有双向链接引用
- 新知识冲突已正确标注

### ⚠️ 黄灯项
- **8 个未同步索引（文件不存在但 index.md 已注册）**：
  - [[Zero_Shot_Prompting]] — 计划中，待创建
  - [[APE_Framework]] — 计划中，待创建
  - [[CO-STAR_Framework]] — 计划中，待创建
  - [[RISEN_Framework]] — 计划中，待创建
  - [[CRAFT_Framework]] — 计划中，待创建
  - [[POWER_Framework]] — 计划中，待创建
  - [[ReAct]] — 计划中，待创建
  - [[Tree_of_Thoughts]] — 计划中，待创建
  - [[RAG]] — 计划中，待创建

- **10 个死链（页面引用不存在的文件）**：
  - [[Constitutional_AI]]（被 Anthropic.md 引用）
  - [[Adaptive_Thinking]]（被 Anthropic.md、Claude.md 引用）
  - [[Agentic_Systems]]（被 Anthropic.md、Context_Engineering.md 引用）
  - [[Token_Efficiency]]（被 5C_Framework.md、摘要-5c-prompt-contracts-paper.md 引用）
  - [[Prompt_Design]]（被 5C_Framework.md、摘要-5c-prompt-contracts-paper.md 引用）
  - [[DSL_Prompting]]（被 5C_Framework.md 引用）
  - [[ChatGPT]]（被 摘要-complete-prompt-engineering-guide-2025.md 引用）
  - [[DeepMind]]（被 Google.md 引用）

### ❌ 红灯项
- **1 个待解决的知识冲突**：
  - [[5C_Framework]] 页面中记录的「5C vs 复杂 DSL 之争」（概念性标注，待补充具体冲突内容）

### 🛠️ 下一步行动
1. **高优先级**：创建核心概念页面（Zero_Shot_Prompting、ReAct、RAG）
2. **中优先级**：创建框架页面（APE、CO-STAR、RISEN、CRAFT、POWER）
3. **低优先级**：补充辅助概念（Constitutional_AI、DeepMind、Token_Efficiency 等）
4. **可选**：完善 5C_Framework 的知识冲突区块，补充具体争议点

### 统计
- 总文件数：19 个（不含 index/log）
- 存在页面：19 个
- 孤儿页面：0 个
- 未同步索引：8 个（计划中的框架/技术）
- 死链：10 个（辅助/补充概念）
- 知识冲突：1 个（概念性标注）

---

## [2026-04-17] ingest | 批量摄入CANN昇腾社区Ascend C性能优化文档（第2批）

### 处理文件清单（12篇）

- `raw/01-articles/Ascend C API列表-CANN社区版9.0.0-beta.2-昇腾社区.md` — Ascend C API完整列表
- `raw/01-articles/优化建议总览表-CANN社区版9.0.0-beta.2-昇腾社区.md` — 性能优化建议总览表
- `raw/01-articles/核间负载均衡-CANN社区版9.0.0-beta.2-昇腾社区.md` — 核间负载均衡策略
- `raw/01-articles/设置合适的核数和算子Kernel类型-CANN社区版9.0.0-beta.2-昇腾社区.md` — 核数与Kernel类型设置
- `raw/01-articles/限制TilingData结构大小-CANN社区版9.0.0-beta.2-昇腾社区.md` — TilingData结构优化
- `raw/01-articles/避免TPipe在对象内创建和初始化-CANN社区版9.0.0-beta.2-昇腾社区.md` — TPipe优化与编译器优化
- `raw/01-articles/核函数内删除Workspace相关冗余操作-CANN社区版9.0.0-beta.2-昇腾社区.md` — Workspace冗余操作删除
- `raw/01-articles/使能DoubleBuffer-CANN社区版9.0.0-beta.2-昇腾社区.md` — DoubleBuffer并行优化
- `raw/01-articles/尽量一次搬运较大的数据块-CANN社区版9.0.0-beta.2-昇腾社区.md` — 内存访问带宽优化
- `raw/01-articles/高效的使用搬运API-CANN社区版9.0.0-beta.2-昇腾社区.md` — DataCopyParams参数使用
- `raw/01-articles/避免同地址访问-CANN社区版9.0.0-beta.2-昇腾社区.md` — 512字节对齐冲突规避
- `raw/01-articles/设置合理的L2 CacheMode-CANN社区版9.0.0-beta.2-昇腾社区.md` — L2 Cache命中率优化

### 创建的来源摘要（12个）

| 文件 | 描述 |
|------|------|
| [[摘要-ascend-c-api列表]] | Ascend C API完整列表 |
| [[摘要-ascendc算子优化建议总览表]] | 性能优化建议总览表 |
| [[摘要-核间负载均衡]] | 核间负载均衡策略 |
| [[摘要-设置合适的核数和算子kernel类型]] | 核数与Kernel类型设置 |
| [[摘要-限制tilingdata结构大小]] | TilingData结构优化 |
| [[摘要-避免tpipe在对象内创建和初始化]] | TPipe优化 |
| [[摘要-核函数内删除workspace相关冗余操作]] | Workspace冗余操作删除 |
| [[摘要-使能doublebuffer]] | DoubleBuffer并行优化 |
| [[摘要-尽量一次搬运较大的数据块]] | 内存访问带宽优化 |
| [[摘要-高效的使用搬运api]] | DataCopyParams参数使用 |
| [[摘要-避免同地址访问]] | 512字节对齐冲突规避 |
| [[摘要-设置合理的l2-cachemode]] | L2 Cache命中率优化 |

### 优化分类总结
- **Tiling策略**：核间负载均衡
- **头尾开销优化**：核数设置、TilingData精简、TPipe优化、Workspace删除
- **流水编排**：DoubleBuffer机制
- **内存访问**：大块数据传输、搬运API优化、同地址规避、L2 CacheMode设置

### 更新文件
- [[index.md]] — 需同步新来源摘要
- [[log.md]] — 记录本次操作（本条目）

---

## [2026-04-17] ingest | 创建CANN昇腾Ascend C实体和概念页面

### 创建的实体页面（4个）

| 页面 | 描述 |
|------|------|
| [[CANN]] | 华为昇腾计算架构 |
| [[昇腾]] | 华为AI处理器品牌 |
| [[Ascend C]] | CANN下的C/C++算子开发API |
| [[AI Core]] | 昇腾处理器的核心计算单元 |

### 创建的概念页面（17个）

| 页面 | 优先级 | 描述 |
|------|--------|------|
| [[DoubleBuffer]] | 中 | 基于MTE与Vector队列并行性 |
| [[Tiling策略]] | 高 | 数据块切分策略 |
| [[L2_Cache切分]] | 高 | L2 Cache命中率优化 |
| [[核间负载均衡]] | 中 | 尾核均匀分布 |
| [[头尾开销优化]] | 中 | 头尾开销降低 |
| [[Unified_Buffer融合]] | 高 | UB Buffer融合减少GM搬入搬出 |
| [[AtomicAdd]] | 中 | Matmul结果累加到GM |
| [[Counter模式]] | 高 | Vector mask模式优化 |
| [[Bias_Table_Buffer]] | 高 | BT Buffer实现bias计算 |
| [[Fixpipe_Buffer]] | 高 | FP Buffer实现随路量化 |
| [[L0C_Buffer]] | 高 | L0C暂存实现矩阵乘累加 |
| [[归约操作]] | 高 | BlockReduceSum+WholeReduceSum |
| [[同地址访问冲突]] | 高 | 512字节对齐冲突规避 |
| [[DataCopyParams]] | 高 | 搬运API参数配置 |
| [[TQueBind]] | 高 | VECIN与VECOUT绑定 |
| [[Scalar常量折叠]] | 中 | 编译器优化技术 |
| [[Workspace]] | 中 | 工作空间冗余操作删除 |

### 更新文件
- [[index.md]] — 新增CANN昇腾生态实体和Ascend C性能优化概念
- [[log.md]] — 记录本次操作（本条目）

---
