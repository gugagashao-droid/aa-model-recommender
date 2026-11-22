# aa-model-recommender

简体中文 | [English](README.md)

一个 agent 技能，回答一个问题：**"这件事到底该用哪个大模型？"**

你描述要做的活。技能先反问几个关键问题，把宽泛任务拆成原子模块，给每个模块映射到最相关的公开榜单，实时抓取当前排名，然后为每个模块输出 Top3 和推荐理由，最后给出跨模块的综合建议。

> 版本 1.1.0。技能本体的说明是中文写的，README 为中英双语。

## 它做什么

1. **环境检查。** 只读脚本确认本地 Python 运行时。
2. **判定粒度。** "写个快排"是原子任务，"做个网站""做条短视频"是宽泛任务。
3. **反问细化。** 最多两批、每批最多四题：场景、执行方式、优先级（效果 / 平衡 / 成本 / 速度）、部署限制。
4. **拆解宽泛任务**，拆到每个模块都能对应到一张榜，例如 UI 审美、前端代码、代码评审、运维。
5. **实时抓榜**，记录每张榜的数据日期和来源链接。
6. **逐模块排序**，按你的优先级和约束过滤，再给出"主模型加替换模块"这类综合建议。

## 数据来源

| 能力 | 来源 |
|---|---|
| 综合智能、编码、agent、长上下文、防幻觉 | [Artificial Analysis](https://artificialanalysis.ai) 各子榜，经聚合站（benchleader、modelcap、llmboard、benchlm）读取 |
| 图像、视频、语音、音乐 Arena | Artificial Analysis 的 Arena 页面 |
| UI 与网页设计 | [Contra Frontier Design](https://contralabs.com/frontierdesign) |
| 写作（中文、英文） | LM Arena 的类别（经镜像站），加 aiwritingbenchmark.com |

所有排名都在运行时现抓。**仓库里没有技能依赖的榜单快照。** `tests/` 下的两个文件含带日期的示例，仅作演示。

榜单数据归各自所有者所有。本项目只链接它们，并在注明来源和日期的前提下引用少量结果片段。

## 安装

把 `aa-model-recommender/` 文件夹复制到你的 agent 的技能目录。例如 Claude Code 从 `~/.claude/skills/` 读取个人技能：

```bash
cp -r aa-model-recommender ~/.claude/skills/
```

其他 agent 有各自的技能目录。技能要求宿主具备两种能力：打开网址并读取页面文字，以及联网搜索。不需要 API Key，也不需要登录。

可以这样试一下：

```text
我想做条短视频，该用哪个模型？
```

## 缺数据时怎么办

- 抓取工具连不上某个站点时，技能会改用本机的只读脚本 `scripts/fetch_page_text.py`，它可以使用你的系统代理。脚本只允许 HTTPS、固定的站点白名单和 GET 请求。
- 仍然失败，就请你在自己的浏览器里打开页面，把表格或截图贴过来。
- 某项能力没有达标的公开榜（例如文字润色），技能会如实说明并建议你做小样本自测，不会编造排名。
- 外部榜要先过置信度门槛（来源权威性、方法披露、新鲜度、样本量、交叉验证），每条结果都会写明置信度和局限。

## 已知局限

- **触发行为没有在真实 agent 会话里验证过。** `tests/trigger-tests.md` 只是静态分析。
- 有些榜测的是相近能力，不是精确能力（例如用 Terminal-Bench 代理视频渲染）。输出里会标注为代理。
- 写作类榜单以英文为主。中文写作用 LM Arena 的中文类别，它的名次区间很宽，前几名常常实际上并列。
- 价格来自第三方快照，只含输出价，速度数据经常缺失。
- 排名反映的是基准测试表现，不保证在你自己的任务上同样好。建议用前几名在小样本上实测。
- `aa-benchmarks.md` 里的榜单名称和网址可能变化，抓取失败时请重新核对。

## 仓库结构

```text
.
├── README.md
├── README.zh-CN.md
├── LICENSE
└── aa-model-recommender/
    ├── SKILL.md                 工作流（Step 0 到 Step 5）
    ├── skill-dependencies.json  依赖与降级映射
    ├── references/              榜单清单、抓取规则、输出格式、反问库
    ├── scripts/                 check_environment.py、fetch_page_text.py
    └── tests/                   触发分析与示例对比
```

## 参与贡献

欢迎提 Issue 和 Pull Request，尤其是：

- 新发现的榜单，需要公开方法、样本量和日期。
- 失效网址的修正。
- 真实会话里的触发测试结果。

## 许可证

[MIT](LICENSE)
