## Howdy y'all

I'm Nico — Lead Software Engineer at [@Resideo](https://github.com/resideo), based in Austin.

Been building software for over twenty years now. Started with PHP and MySQL back in '05, picked up iOS development early on, and it stuck. Spent time at Match, Shyp, Synack, and American Airlines before landing at Resideo, where I lead mobile engineering. Most of my work these days is in Flutter.

### What I'm tinkering with

**Multi-agent orchestration (the factory loop)**

Most of my weekend hacking lately is on autonomous agent workflows — a pipeline where AI agents pick up work items (called *beads*), write blueprints, implement code, review each other's PRs, and land changes with minimal human input. A supervisor coordinates the whole thing: assigns priorities, resolves blockers, and keeps the factory moving.

Some live numbers from the hobby bench:

- **65** beads tracked across projects
- **~88%** completion rate (57 closed)
- **310** commits landed across factory-managed repos
- **5** unique contributors (a mix of human + agent identities)

Built on top of [OpenClaw](https://github.com/openclaw/openclaw) with custom orchestration layers. Still very much weekend-science-experiment energy, but the agents are getting surprisingly productive.

**Local inference on Apple Silicon**

Also building a local inference orchestration server in Swift — runs multiple model backends (MLX, FlashMoE) behind a single OpenAI-compatible API and routes requests to whichever node makes sense. Early telemetry from real requests on an M-series Mac:

| Metric | Value |
|--------|-------|
| Avg time-to-first-token | **331 ms** (p50: 117 ms) |
| Avg throughput | **26.7 tokens/sec** |
| Peak local throughput | **~65 tokens/sec** (7B model on MLX) |
| Backends tested | MLX (7B, 32B), FlashMoE (397B) |

Still early days — the goal is to publish reproducible benchmarks for different model sizes on consumer Apple hardware.

🤖 - [OpenClaw](https://github.com/openclaw/openclaw)

---

*Last updated: March 29, 2026*
