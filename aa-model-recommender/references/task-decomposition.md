# 宽泛任务拆解为原子模块

本文件定义如何把用户的宽泛任务拆成可单独匹配子榜、单独推荐模型的原子模块。这是"真正推荐模型"的关键:一个宽泛任务(如"做个网页")不是一个模型能最优完成的,必须拆开按模块选。

## 一、判定任务粒度

先判断用户输入属于哪一类:

- **原子任务**:单一、具体的执行目标,能直接对应到一组 AA 子榜。例:"帮我写一段快排 Python"、"把这个 PDF 总结成一页"、"解释一下相对论"。
  → 直接进入子榜映射,不拆解。
- **宽泛任务**:包含多个阶段、多种能力、或多个产出物。特征信号:"做个/开发个 X"、"写篇论文"、"做个 App"、"做个数据分析"、"帮我从零搭建 X"、描述里出现多个动词或名词产物。
  → 必须先按本文件拆解,再逐模块推荐。

判断不确定时,倾向于当作宽泛任务处理并说明,宁可多拆不要漏拆。

## 二、拆解原则

1. **MECE**:各模块相互独立、整体穷尽,不重叠、不遗漏主要工作。
2. **可评估性**:每个叶子模块尽量能对应到至少 1 张榜(AA 子榜或经审核的外部榜,用第三节的能力→子榜表核对)。若某叶子对应不到,先尝试下钻或合并到能对应的模块;确实没有达标公开榜的(如润色),**保留该叶子并标为"无榜"**,输出时如实说明,不要硬凑也不要删掉。
3. **同质性**:一个叶子模块内能力同质(都是写代码、都是长文档阅读、都是审美设计),不要把"写代码"和"做设计"塞进同一叶子。
4. **深度可控**:一般拆 3–8 个叶子模块;超过 8 个时,按重要性排序并与用户确认聚焦哪几个,避免输出过长。
5. **视角维度要显式**:用户特别强调的视角(如"前端不仅写代码还要审美设计""后端不仅写代码还要 review 和架构")必须拆成独立叶子,不能被"前端开发""后端开发"一个词吞掉。

## 三、通用能力 → 子榜对照(拆叶子时用于验证可评估性)

"衡量关系"一列说明榜单与能力的关系:**直接** = 榜单就是在测这个能力;**代理** = 没有专门榜,借用最接近的信号;**无榜** = 没有达到置信门槛的公开榜。

| 原子能力 | 首选子榜(slug) | 补充 | 衡量关系 |
|---|---|---|---|
| 写业务代码 / 改 bug | `aa_frontiercode_20`、`aa_terminalbench_40` | Coding Agent Index、`aa_livecodebench` | 直接 |
| 代码评审 / review | `aa_frontiercode_20`(合并判定)、`aa_terminalbench_40` | `aa_cyber_index`(安全向 review) | **代理**:没有专门的 review 榜,合并判定是最接近的信号 |
| 系统架构 / 技术设计 | `aa_intelligence_index_v5`、`aa_gdpval` | `aa_hle_diamond`(复杂推理) | **代理**:综合推理能力,不是架构设计专项 |
| UI 审美与页面实现(模型直接生成页面) | **Contra Frontier Design**(外部榜,高置信,见 `aa-benchmarks.md` 第五节) | `aa_mmmu_pro`(视觉理解,代理) | 直接(外部榜) |
| 配图 / 素材 / 海报出图 | 官方 Image Arena(Text to Image / Image Editing) | `aa_3d` | 直接。**不能代替 UI 页面设计** |
| 前端界面还原 / 视觉到代码 | `aa_mmmu_pro`、`aa_frontiercode_20` | Contra Frontier Design | **代理** |
| 部署 / 运维 / SRE | `aa_itbench_sre`、`aa_terminalbench_40` | `aa_enterprise_ops_gym`、`aa_automationbench` | 直接 |
| 自动化测试 / 终端任务 | `aa_terminalbench_40`、`aa_terminalbench_21` | `aa_scicode` | 直接 |
| 长文档阅读 / 文献综述 | `aa_lcr`、`aa_gdp_pdf` | `aa_mlcr` | 直接 |
| 数据/表格分析 | `aa_analyst_agent`、`aa_gdpval` | `aa_gdp_pdf` | 直接 |
| 科学计算 / 科研代码 | `aa_scicode` | `aa_frontiercode_20` | 直接 |
| 学术/专业推理论证 | `aa_hle_diamond`、`aa_gpqa`、`aa_critpt` | 领域榜见 `aa-benchmarks.md` | 直接 |
| 事实核查 / 防幻觉 / 引用 | `aa_omniscience`、`aa_omniscience_accuracy` | `aa_omniscience_non_hallucination` | 直接 |
| 写作质量(英文创作、文风、专业文体) | **外部榜**:LM Arena 创意写作类别 + aiwritingbenchmark 分类榜,两源交叉 | AA-PersonalityBench 仅作文风与人格倾向参考 | 直接(外部榜,中置信,必须交叉验证) |
| 中文写作 | **外部榜**:LM Arena 中文类别 | LM Arena 创意写作类别作参考;aiwritingbenchmark 没有中文版 | 直接(外部榜,中置信;名次区间宽,前几名常接近) |
| 学术写作 / 论文写作 | **代理**:LM Arena"写作与文学行业"类别 + aiwritingbenchmark 专业文书分类 | SciIG、ArXivBench 只作方法参考 | **代理**:没有现行的专门学术写作榜 |
| 润色 / 改写 | **无达标公开榜** | `aa_ifbench` 只用于"格式遵循",不代表润色质量 | **无榜**:如实告知,建议小样本自测 |
| 配乐 / BGM / 主题曲 / 歌曲生成 | 官方 AA-Music-Instrumental / AA-Music-Vocal(见 `aa-benchmarks.md` Arena 表) | 学术界 AIME 人类偏好榜(交叉验证) | 直接 |
| 结构化输出 / 严格格式 / 格式规范遵循 | `aa_ifbench`、`aa_vials` | — | 直接 |
| 需求梳理 / 决策判断 | `aa_intelligence_index_v5`、官方 Decision Index(尚无已核实直链) | `aa_gdpval` | **代理** |
| 找信息 / 检索 agent | 官方 Search API 榜(尚无已核实直链)、`aa_apex_agents` | `aa_omniscience` | 部分直接 |

**对"代理"和"无榜"叶子的处理规则:**

- 标"代理"的叶子,输出时必须写明"该榜是代理指标,不直接衡量该能力",并把结论措辞放软。
- 标"无榜"的叶子,不得硬凑 Top3;走 `fetch-guide.md` 第四节第三级的保守建议。
- 多个叶子落在同一张子榜上(例如前端开发、视觉还原、代码评审都落在 `aa_frontiercode_20`),它们的排名相同。输出时合并成一张表,并说明"这些模块共用该榜",而不是假装三个模块分别评出了冠军。

## 四、常见宽泛任务拆解模板

以下为默认骨架,实际使用须结合第五节反问结果裁剪、下钻或增加模块。

### 模板 A:做个网页 / 网站 / Web 应用

| 叶子模块 | 核心能力 | 候选子榜 |
|---|---|---|
| 需求梳理与信息架构 | 需求→结构/决策 | `aa_intelligence_index_v5`、Decision Index |
| UI 审美与页面实现 | 模型直接生成好看的页面 | Contra Frontier Design(外部榜) |
| 配图 / 素材出图(按需) | 出图 | Image Arena、`aa_3d` |
| 前端开发 | 写界面代码 | `aa_frontiercode_20`、`aa_terminalbench_40` |
| 前端视觉还原 | 视觉理解+代码 | `aa_mmmu_pro`、`aa_frontiercode_20` |
| 后端开发 | 写服务端代码 | `aa_frontiercode_20`、`aa_terminalbench_40` |
| 架构与技术选型 | 系统设计 | `aa_intelligence_index_v5`、`aa_gdpval` |
| 代码评审 / review | 合并判定、质量 | `aa_frontiercode_20`、`aa_cyber_index` |
| 测试 / 部署 / 运维 | 终端与 SRE | `aa_terminalbench_40`、`aa_itbench_sre`、`aa_enterprise_ops_gym` |
| 文案 / 内容 | 写作表达 | 外部写作榜(中文用 LM Arena 中文类别;英文用 aiwritingbenchmark 营销分类 + LM Arena 创意写作) |

> 反问结果会裁剪:纯静态展示站可去掉"后端/架构/运维";电商/后台系统则必须保留并加重 review、架构、运维。

### 模板 B:写论文 / 学术写作

| 叶子模块 | 核心能力 | 候选子榜 |
|---|---|---|
| 选题与研究方向 | 推理/决策 | `aa_hle_diamond`、Decision Index、领域榜 |
| 文献综述 / 长文阅读 | 长上下文 | `aa_lcr`、`aa_gdp_pdf` |
| 实验设计 / 数据分析 | 表格文档分析 | `aa_analyst_agent`、`aa_gdpval` |
| 科研代码 / 实验实现 | 科学计算代码 | `aa_scicode`、`aa_frontiercode_20` |
| 论证与专业推理 | 学科推理 | `aa_gpqa`、`aa_critpt`、`aa_hle_diamond`、领域榜 |
| 事实核查 / 引用准确性 | 防幻觉 | `aa_omniscience`、`aa_omniscience_accuracy` |
| 学术写作 / 结构 | 学术写作 | LM Arena"写作与文学行业"类别、aiwritingbenchmark 专业文书分类(外部榜,代理);中文论文另看 LM Arena 中文类别 |
| 润色 | 改写质量 | 无达标公开榜,小样本自测 |
| 格式 / 规范遵循 | 指令遵循 | `aa_ifbench`、`aa_vials` |

> 反问结果会裁剪:本科生课程论文可弱化"实验代码/专业推理",强化"写作/防幻觉/格式";博士论文或期刊投稿则全程强化,并按专业映射到 `aa_harvey_lab`(法律)、`aa_healthbench_professional`(医学)、`aa_critpt`(物理)等。

### 模板 C:做个 App / 移动应用

需求梳理 → UI 审美(Contra Frontier Design)+ 配图素材(Image Arena) → 客户端开发(coding) → 后端/API(coding) → 架构(intelligence/gdpval) → review(frontiercode) → 测试部署(terminalbench/itbench)。

### 模板 D:做个数据分析 / 报表

需求澄清 → 数据清洗(coding/analyst) → 分析建模(`aa_analyst_agent`、`aa_gdpval`) → 可视化(图表由代码生成时看 coding 榜;只有要出插画类图片时才用 Image Arena) → 结论写作(外部写作榜、`aa_briefcase`) → 核查(`aa_omniscience`)。

### 模板 E:做条短视频 / 视频内容

| 叶子模块 | 核心能力 | 候选子榜 |
|---|---|---|
| 文案脚本 | 写作表达 | 外部写作榜(中文用 LM Arena 中文类别;英文用 aiwritingbenchmark 营销/演讲分类 + LM Arena 创意写作)、`aa_briefcase` |
| 视觉设计 / 出图(素材) | 审美出图 | 官方 Image Arena(Text to Image / Image Editing)、`aa_3d` |
| AI 视频生成 | 文生视频 | AA Text to Video Arena(URL 与抓取梯队见 `aa-benchmarks.md` 第一节) |
| 实拍素材动态化 | 图生视频 | AA Image to Video Arena |
| AI 配音 | 语音合成 | AA Text to Speech Arena |
| 配乐 / BGM | 音乐生成 | AA-Music-Instrumental / AA-Music-Vocal(无镜像,抓不到时请用户代取) |
| 成片/讲解视频 | 分镜到成片 | `aa_code2video` |

> 反问裁剪:画面来源(纯AI生成/实拍混合/图文转视频)决定是否保留"图生视频/出图"模块;音频需求(配音/配乐/无)决定保留哪些音频模块。注意 Arena 类榜官方优先、抓不到按梯队降级(镜像、请用户代取),详见 `fetch-guide.md` 第一节。

### 模板 F:没有现成模板的宽泛任务

按"输入→处理→产出→质量保障→交付"生命周期自行拆分,每个阶段问自己:这里需要哪种原子能力(用第三节表格核对)?是否需要下钻出用户强调的视角维度?然后为每个叶子记录候选子榜。

## 五、拆解后必须做的两件事

1. **逐模块反问细化**:每个叶子模块若仍缺关键信息(规模、技术栈、领域、目标读者等),按 `clarifying-questions.md` 追问,不要凭空假设。
2. **逐模块独立推荐**:每个叶子模块单独走"子榜映射 → 实时抓取 → Top3 推荐",在输出里对每个模块给出该模块的最佳模型,而不是给整个宽泛任务一个笼统答案。最终再给一个"跨模块综合建议"(如是否存在一个模型在多数模块都靠前)。
