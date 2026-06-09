## Howdy y'all

I'm Nico — Lead Software Engineer at [@Resideo](https://github.com/resideo), based in Austin.

Been building software for over twenty years now. Started with PHP and MySQL back in '05, picked up iOS development early on, and it stuck. Spent time at Match, Shyp, Synack, and American Airlines before landing at Resideo, where I lead mobile engineering. Most of my work these days is in Flutter.

### What I'm tinkering with

**Multi-agent orchestration (the factory loop)**

Most of my weekend hacking lately is on autonomous agent workflows — a pipeline where AI agents pick up work items (called *beads*), write specs, implement code, review each other's PRs, and land changes with minimal human input. A supervisor keeps the factory moving through a *discover → specify → committee → build → review → merge* loop, assigning priorities and clearing blockers along the way.

Some live numbers from the hobby bench:

- **1,816** beads tracked across three project trackers
- **~83%** completion rate (1,512 closed)
- **2,080** commits landed across factory-managed repos
- a human plus a rotating cast of agents (one of them, Chad, just got put on ice)

Just spent the past few weeks of nights and weekends ripping out a custom Go implementation and moving the whole thing onto **factoryskills** (the `fs` dev loop) running on the Gas City engine, with **beads** (`bd`) handling work tracking. Still very much weekend-science-experiment energy, but the agents are getting surprisingly productive.

**Local inference on Apple Silicon**

Also building **swift-infer**, a local inference orchestration server in Swift — runs multiple model backends behind a single OpenAI-compatible API and routes each request to whichever node makes sense. Right now it's serving Qwen 3.6 — a 27B dense model and a 35B-A3B MoE — both 8-bit on MLX, all running locally.

Live numbers over the last 500 requests:

| Metric | Value |
|--------|-------|
| Avg time-to-first-token | **86 ms** (p50: 56 ms, p95: 109 ms) |
| Avg throughput | **39.8 tokens/sec** |
| Peak throughput | **~80 tokens/sec** (the 35B-A3B MoE) |

For coding quality I lean on EvalPlus HumanEval — earlier runs through the same server clocked gemma4-fast (31B, 4-bit) at **89.0%** pass@1 and qwen3-coder-next at **90.9%**.

The goal is reproducible benchmarks for different model sizes on consumer Apple hardware.

---

*Last updated: June 8, 2026*
