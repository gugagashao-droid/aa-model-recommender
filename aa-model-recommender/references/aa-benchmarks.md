# AA 测评子榜清单与任务映射

本文件是任务场景 → Artificial Analysis(AA)具体子榜单的映射依据。AA 官方站 `artificialanalysis.ai` 把评测分成若干族群,第三方聚合站(benchleader/modelcap/llmboard/benchlm)把每个子榜做成独立页面,便于实时抓排名。

> 宽泛任务需先拆成原子模块再映射子榜,拆解方法与"原子能力 → 子榜对照"见 `task-decomposition.md`;场景化反问见 `clarifying-questions.md`。本文件第二节的场景映射用于原子任务或拆解后的模块归入场景时使用。

## 一、AA 综合指数( composites,优先作为通用任务主榜 )

| 榜单 | slug | 衡量内容 | 适用场景 |
|---|---|---|---|
| AA Intelligence Index v5.0.1 | `aa_intelligence_index_v5` | 9 项评测复合:AA-Briefcase v1.1、AutomationBench-AA、Terminal-Bench 4.0、FrontierCode-AA 2.0、AA-Omniscience v1.1、GDP.pdf、HLE-Diamond、CritPt v2(Hard)、Terminal-Bench-Science | **通用任务的默认主榜**;说不清场景、综合工作能力 |
| AA Intelligence Index(旧 v4.x) | `aa_intelligence_index` | 上一代复合 | 兼容旧数据对比 |
| AA Coding Agent Index | 官方 Coding Agents 区 | FrontierCode-AA 2.0 + Terminal-Bench 4.1 + SWE-Atlas-QnA-AA 平均 pass@1 | 端到端软件工程/编码 agent |
| AA Cyber Index | `aa_cyber_index` | CWE-Bench-AA + DeepsecBench-AA + CyberGym-E2E-AA | 网络安全 |
| AA Decision Index | 官方 Decision Index 区 | AA-Paperclip 等多项决策评测加权 | 决策/判断类任务 |
| AA Capability Indexes | 官方 Capability Indexes 区 | 财务会计/战略运营/法律/医疗/工程/经济 分行业 | 垂直行业能力 |
| AA Progress Index v1 | `aa_progress_index` | 跨时间能力估计 | 趋势观察(非选型) |
| AA Openness Index | `aa_openness_index` | 模型开放度(最高 18) | 开源/透明度评估 |

> 以上"官方 Capability Indexes / Coding Agent Index / Decision Index"在聚合站没有独立 slug,**本技能尚未核实它们的官方直链**(核验时 artificialanalysis.ai 连接被拒)。使用时先用搜索定位官方页面;定位不到就请用户在浏览器里打开并贴出表格,仍不行再按 `fetch-guide.md` 第四节降级,不要猜 URL。

### Arena 类榜单(AA 自家盲投偏好 Elo)

Arena 类榜单(Music / Image / Video / Speech)**不走 benchleader 的 `aa_*` slug 体系**,是第三节"47 个 slug"清单之外的补充。官方页面可能带人机验证或在部分网络下连不上,**官方优先,抓不到按 `fetch-guide.md` 第一节的梯队降级**(官方页 → 本机命令行抓取 → GitHub 每日快照镜像 → 搜索摘要 → 请用户代取):

| 榜单 | 官方 URL | 衡量内容 | 适用场景 | 有无镜像 |
|---|---|---|---|---|
| AA-Music-Vocal v1.1 | `https://artificialanalysis.ai/music/leaderboard/vocals` | 带人声歌曲质量,盲听偏好 Elo | **歌曲生成、主题曲、带人声配乐** | 无 |
| AA-Music-Instrumental v1.1 | `https://artificialanalysis.ai/music/leaderboard/instrumental` | 纯音乐/伴奏质量,盲听偏好 Elo | **纯配乐/BGM/背景音乐** | 无 |
| Text to Image | `https://artificialanalysis.ai/image/leaderboard/text-to-image` | 文生图偏好 Elo | 配图、素材、海报(**不能代替 UI 页面设计**) | 有 |
| Image Editing | `https://artificialanalysis.ai/image/leaderboard/image-editing` | 图像编辑偏好 Elo | 改图、换背景 | 有 |
| Text to Video | `https://artificialanalysis.ai/video/leaderboard/text-to-video` | 文生视频偏好 Elo | 短视频素材 | 有 |
| Image to Video | `https://artificialanalysis.ai/video/leaderboard/image-to-video` | 图生视频偏好 Elo | 让静态素材动起来 | 有 |
| Text to Speech | `https://artificialanalysis.ai/text-to-speech/leaderboard/provider-voice` | 语音合成偏好 Elo | 配音、旁白 | 有 |

> 表中"有镜像"指 GitHub 每日快照镜像存在该榜。2026-11-22 核验时镜像的 `latest.json` 停在 2026-07-10,已过时,只能作交叉验证;实际取数先走官方页或本机命令行抓取(见 `fetch-guide.md` 第一节)。

## 二、按任务场景 → 子榜映射表(核心)

| 用户任务场景 | 首选子榜(slug) | 补充/备选子榜 |
|---|---|---|
| 写代码、改 bug、软件工程、仓库级改动 | `aa_frontiercode_20`、`aa_terminalbench_40`、Coding Agent Index | `aa_scicode`、`aa_livecodebench`、`aa_terminalbench_21/aa_terminalbench_hard`、`aa_frontiercode` |
| 生成文档/表格/PPT/报告、知识工作产出 | `aa_briefcase` | `aa_gdp_pdf`、`aa_gdpval`/`aa_gdpval_elo`、`aa_analyst_agent` |
| 自动化流程、SaaS 工作流、工具调用、多步 agent | `aa_automationbench`、`aa_tau2` | `aa_apex_agents`、`aa_enterprise_ops_gym`、`aa_tau_banking`、`aa_itbench_sre` |
| 数学/物理/科学推理、难题解答 | `aa_hle_diamond`、`aa_critpt_hard` | `aa_gpqa`、`aa_hle`、`aa_aime_2025`、`aa_global_mmlu_lite` |
| 长文档/长上下文阅读理解 | `aa_lcr` | `aa_mlcr`(医疗长文本) |
| 事实问答、防幻觉、可靠性 | `aa_omniscience` | `aa_omniscience_accuracy`、`aa_omniscience_non_hallucination` |
| 图片/视频/语音生成 | 上文 Arena 类榜单表(Image/Video/Speech) | `aa_code2video` |
| UI 审美、网页/应用视觉设计(模型生成页面) | **外部榜**:Contra Frontier Design(第五节,高置信) | Image Arena 仅用于"配图素材"子模块;`aa_mmmu_pro`(视觉理解,代理) |
| 金融/会计 | Capability Indexes(Finance & Accounting) | `aa_gdpval`、`aa_analyst_agent` |
| 法律 | `aa_harvey_lab` | Capability Indexes(Legal) |
| 医疗 | `aa_healthbench_professional` | Capability Indexes(Healthcare)、`aa_mlcr` |
| 网络安全 | `aa_cyber_index` | `aa_cwe_bench`、`aa_deepsec_bench`、`aa_cybergym_e2e` |
| 视觉理解/多模态推理 | `aa_mmmu_pro` | `aa_mka` |
| 3D/交互内容 | `aa_3d` | — |
| 指令遵循、格式要求严格 | `aa_ifbench` | `aa_vials` |
| 写作质量(英文创作/文风/专业文体) | **外部榜**:LM Arena 创意写作类别 + aiwritingbenchmark 分类榜,两源交叉(第五节) | AA-PersonalityBench 只能说明**文风与人格倾向**,不能当写作质量依据 |
| 中文写作 | **外部榜**:LM Arena 中文类别(第五节) | 另看创意写作类别作参考;aiwritingbenchmark 没有中文版 |
| 学术写作/论文写作 | **代理**:LM Arena"写作与文学行业"类别 + aiwritingbenchmark 专业文书分类(第五节) | 没有专门的现行学术写作榜;SciIG、ArXivBench 只作方法参考 |
| 润色/改写 | **无达标公开榜**(第五节说明) | 如实告知,给保守建议 |
| 配乐/背景音乐/BGM/主题曲 | AA-Music-Instrumental、AA-Music-Vocal(见上文 Arena 类榜单表) | 视频配乐、歌曲生成走 Vocal 赛道 |
| 通用/说不清 | `aa_intelligence_index_v5` | 按描述补充上面某个集群 |
| 成本/速度/开源约束 | 见 `fetch-guide.md` 价格·速度·权重维度 | `aa_openness_index` |

## 三、全部 AA 子榜 slug 索引(47 个,来自 benchleader `/benchmarks`)

`aa_3d`、`aa_aime_2025`、`aa_analyst_agent`、`aa_apex_agents`、`aa_automationbench`、`aa_briefcase`、`aa_code2video`、`aa_critpt`、`aa_critpt_hard`、`aa_cwe_bench`、`aa_cyber_index`、`aa_cybergym_e2e`、`aa_deepsec_bench`、`aa_enterprise_ops_gym`、`aa_frontiercode`、`aa_frontiercode_20`、`aa_gdp_pdf`、`aa_gdpval`、`aa_gdpval_elo`、`aa_global_mmlu_lite`、`aa_gpqa`、`aa_harvey_lab`、`aa_healthbench_professional`、`aa_hle`、`aa_hle_diamond`、`aa_ifbench`、`aa_intelligence_index`、`aa_intelligence_index_v5`、`aa_itbench_sre`、`aa_lcr`、`aa_livecodebench`、`aa_mka`、`aa_mlcr`、`aa_mmmu_pro`、`aa_omniscience`、`aa_omniscience_accuracy`、`aa_omniscience_non_hallucination`、`aa_openness_index`、`aa_progress_index`、`aa_scicode`、`aa_tau2`、`aa_tau_banking`、`aa_terminalbench_21`、`aa_terminalbench_40`、`aa_terminalbench_hard`、`aa_terminalbench_science`、`aa_vials`

> 拼接规则:`https://www.benchleader.com/benchmarks/<slug>` 即得该子榜页面。官方对应页多为 `https://artificialanalysis.ai/evaluations/<短名>`(如 `aa-briefcase`)。

## 四、重要口径提醒

- **分数方向**:绝大多数子榜分数越高越好;AA-WER 语音识别是越低越好;Elo 类(AA-Briefcase、GDPval-AA)是相对分而非百分比。做 Top3 排序时统一按"越高越好"处理,Elo 直接用数值比较。
- **版本**:优先用带版本号的榜(如 `aa_intelligence_index_v5`、`aa_briefcase` v1.1、`aa_omniscience` v1.1);同一模型的多 effort 变体按最佳配置记录。
- **证据等级**:标注为"lab-reported / 自报"或证据等级 C 的条目,可靠性低于独立测评(B),推荐时优先独立测评结果。

## 五、非 AA 外部榜单(AA 没有直接对应榜的能力)

这些榜不属于 AA,使用前必须过 `fetch-guide.md` 第五节置信度门槛,并在输出里写明"外部榜"、置信度和局限。核验日期 2026-11-22;抓取时数据日期以榜页当时显示为准。**本节不记录具体名次和分数,运行时必须重抓。**

### UI 审美与页面实现:Contra Frontier Design(Contra Labs × BenchFlow)

- **榜单页**:`https://contralabs.com/frontierdesign/leaderboard`;方法说明 `https://contralabs.com/frontierdesign/methodology`;评分细则 `https://contralabs.com/frontierdesign/rubric`。核验时用网页抓取工具读到了文本表格,含名次、Elo、胜率、95% 区间和票数。
- **衡量内容**:模型根据真实客户简报生成可运行网站,由职业设计师在隐藏模型名的前提下两两盲投,换算成 Elo;人类设计稿同表参照。
- **规模**:33 份简报(27 份改编自 Contra 平台真实需求),4,311 票。页面对设计师人数有两处不同说法,使用时不引用该数字。
- **置信度:高(维护者指定)**。按第五节门槛表,它的"交叉验证"一栏是单一来源,本应只到中;维护者确认直接按高置信使用。输出时仍如实写出样本量(33 份简报)和"单一来源",只是不再因此降级。
- **页面日期**:只写"Updated Nov 2026",按 11 月 1 日起算。
- **进 Top3 前必须剔除**:匿名隐藏模型(核验时是 Gestalt,无法调用)和人类对照组(Human designers);可在备注中提及。
- **名次接近要说明**:前列模型的 Elo 差几十分以内、95% 区间重叠时,写"接近",不硬排先后。
- **适用范围**:UI/网页的视觉设计与页面实现。它测的是**模型生成的完整网站**,不测代码质量;代码质量仍看 `aa_frontiercode_20` 等。配图素材生成不归它管,走 Image Arena。

### 写作质量

AA 没有写作质量榜(AA-PersonalityBench 衡量风格和人格倾向)。写作榜之间排名差异很大,**必须两个以上来源交叉验证**,榜首不一致时按 `fetch-guide.md` 第五节处理分歧。

| 榜单 | URL | 方法 | 置信度与局限 |
|---|---|---|---|
| LM Arena 创意写作类别 | 官方 `https://arena.ai/leaderboard/text/creative-writing`(自动抓取返回 403);镜像 `https://www.aitntnews.com/arena/text/creative_writing/` | 真实用户匿名盲投 Elo,镜像声明沿用官方口径 | **中**。镜像来源;核验时同步日期 2026-11-11;多数行票数显示 0(视为未披露);不少新模型带 preliminary 标记,Top3 里剔除或注明;Surge 批评此类投票偏爱排版漂亮的文字(对手说法) |
| LM Arena 中文类别 | 镜像 `https://www.aitntnews.com/arena/text/chinese/` | 同上,中文提示的盲投 Elo | **中**。同上;票数有披露(几十到几千);名次区间很宽、名次不稳定,前几名常接近并列,写"接近"而不硬排 |
| LM Arena 写作与文学行业类别 | 镜像 `https://www.aitntnews.com/arena/text/industry_writing_and_literature_and_language/` | 同上,按行业分类的盲投 Elo | **中**。同上;票数显示 0;这是**学术写作的代理**,不是学术写作榜 |
| The AI Writing Benchmark | `https://aiwritingbenchmark.com/`(另有法、德、西班牙语版本,**无中文版**) | 盲测两两对决,页面称 138,706 票、122 个模型,更新到 2026-11-20 | **中**。个人独立项目,但披露了样本量和更新日期;未写明裁判是谁、提示库如何构建、评分公式;有分类榜(小说、邮件、演讲、营销、专业文书等),按场景取分类榜而非总榜 |

**学术写作**:没有找到现行的专门榜单。已知的 SciIG(EMNLP 2026,只评 AI/ML 论文的引言生成)和 ArXivBench(引用可靠性)覆盖面窄,比较的模型也偏旧,只作方法参考。落地做法是用"写作与文学行业"类别和 aiwritingbenchmark 的专业文书分类做**代理**,并在输出里写明"该榜不直接衡量学术写作"。事实核查与引用准确性另看 `aa_omniscience`。

**不建议作为排名依据**:
- WritingBench:论文首发 2025 年 3 月、最新修订 2025 年 11 月,按第五节的 180 天规则只能做方法参考;核验时也没有读到它的榜单页。
- EQ-Bench:大模型自动打分,Surge 称其与专家一致率最低约 43%(对手公司说法,未独立核实),按低置信处理。
- Hemingway-bench(Surge):职业作家盲评、5,000+ 对比,方法最严,但由数据服务商自己运营、页面日期有 2026-02-04 与 2026-10-21 两处、榜首是较旧模型,**只作方法参考,不用其名次**。
- **润色/改写**:没有找到达到置信门槛的公开榜。按 `fetch-guide.md` 第四节第三级处理:如实告知"无达标榜单",建议让用户拿 3–5 段自己的稿子,在候选模型上做小样本盲比。"格式/规范遵循"仍可用 `aa_ifbench`。
