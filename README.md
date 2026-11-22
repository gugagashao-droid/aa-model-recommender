# aa-model-recommender

[简体中文](README.zh-CN.md) | English

An agent skill that answers one question: **"Which LLM should I use for this job?"**

You describe the work. The skill asks a few clarifying questions, splits a broad task into atomic modules, maps each module to the most relevant public leaderboard, fetches the current ranking live, and returns a Top 3 with reasons for each module plus a cross-module recommendation.

> Version 1.1.0. The skill's instructions are written in Chinese. The README is bilingual.

## What it does

1. **Check the environment.** A read-only script confirms the local Python runtime.
2. **Judge granularity.** "Write a quicksort" is atomic. "Build a website" or "make a short video" is broad.
3. **Ask clarifying questions.** At most two batches of four questions: scenario, execution mode, priority (quality / balanced / cost / speed), deployment limits.
4. **Decompose broad tasks** into modules that can each be matched to a leaderboard, for example UI aesthetics, front-end code, code review, ops.
5. **Fetch live rankings** for each module and record the data date and source URL.
6. **Rank per module** by your priority and constraints, then give a cross-module plan such as "main model plus replacements".

## Where the data comes from

| Capability | Source |
|---|---|
| General intelligence, coding, agents, long context, hallucination | [Artificial Analysis](https://artificialanalysis.ai) sub-leaderboards, read through aggregators (benchleader, modelcap, llmboard, benchlm) |
| Image, video, speech, music arenas | Artificial Analysis arena pages |
| UI and web design | [Contra Frontier Design](https://contralabs.com/frontierdesign) |
| Writing (English, Chinese) | LM Arena categories via a mirror, plus aiwritingbenchmark.com |

All rankings are fetched at run time. **The repository contains no ranking snapshot that the skill relies on.** The two files under `tests/` contain dated examples for illustration only.

Benchmark data belongs to its owners. This project only links to it and quotes short result fragments with the source and date.

## Install

Copy the `aa-model-recommender/` folder into the skills directory of your agent. For example, Claude Code reads personal skills from `~/.claude/skills/`:

```bash
cp -r aa-model-recommender ~/.claude/skills/
```

Other agents have their own skills directory. The skill needs two capabilities from the host: open a URL and read the page text, and search the web. It needs no API key or login.

Try it with a prompt such as:

```text
I want to make a short video. Which model should I use?
```

## How it handles missing data

- If the fetch tool cannot reach a site, the skill tries a local read-only helper, `scripts/fetch_page_text.py`, which can use your system proxy. It allows only HTTPS, a fixed host allow-list, and GET requests.
- If that also fails, it asks you to open the page in your browser and paste the table or a screenshot.
- If no qualified public leaderboard exists for a capability (for example, text polishing), it says so and suggests a small test of your own. It does not invent a ranking.
- External leaderboards pass a confidence gate (source authority, method disclosure, freshness, sample size, cross-validation). Each result states its confidence and limits.

## Known limitations

- **Triggering has not been verified in a real agent session.** `tests/trigger-tests.md` is a static analysis only.
- Some leaderboards measure a nearby skill, not the exact one (for example, Terminal-Bench for video rendering). The output labels these as proxies.
- The writing leaderboards are mostly English. Chinese writing uses the LM Arena Chinese category, which has wide rank intervals, so top ranks are often effectively tied.
- Prices come from a third-party snapshot and cover output tokens only. Speed data is often missing.
- Rankings describe benchmark performance. They do not guarantee results on your own task. Test the top candidates on a small sample.
- The skill file `aa-benchmarks.md` lists leaderboard names and URLs that can change. Re-check them when a fetch fails.

## Repository layout

```text
.
├── README.md
├── README.zh-CN.md
├── LICENSE
└── aa-model-recommender/
    ├── SKILL.md                 workflow (Step 0 to Step 5)
    ├── skill-dependencies.json  dependencies and degradation map
    ├── references/              leaderboard list, fetch rules, output format, question bank
    ├── scripts/                 check_environment.py, fetch_page_text.py
    └── tests/                   trigger analysis and an illustrative comparison
```

## Contributing

Issues and pull requests are welcome, especially:

- Newly found leaderboards with a published method, sample size, and date.
- Corrections to URLs that no longer work.
- Real-session trigger test results.

## License

[MIT](LICENSE)
