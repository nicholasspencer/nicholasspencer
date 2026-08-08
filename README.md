## Howdy y'all

I'm Nico — Lead Software Engineer at [@Resideo](https://github.com/resideo), based in Austin.

Been building software for over twenty years now. Started with PHP and MySQL back in '05, picked up iOS development early on, and it stuck. Spent time at Match, Shyp, Synack, and American Airlines before landing at Resideo, where I lead mobile engineering. Most of my work these days is in Flutter — and lately, in Dart everywhere else too.

### What I'm tinkering with

**[memento.engineering](https://github.com/memento-engineering)**

Nearly all of my nights-and-weekends work now lives in one place: a small org built around a single bet.

> Apps are built in Flutter. Leonard drives and debugs the apps. Leonard files bugs as beads into the grid. **The grid builds everything.**

It's a long bet on Dart as a full-stack agentic platform — one substrate engine, a debugging harness that drives running programs, and a work-graph orchestrator that spawns coding agents against the ready frontier.

| Repo | What it is |
|------|------------|
| [genesis](https://github.com/memento-engineering/genesis) | The substrate. Flutter's element model extracted to pure Dart — a framework-agnostic `Seed`→`Branch` keyed-reconcile engine, plus perception, taxonomy, an A2UI wire, and a terminal render backend. On pub.dev. |
| [lenny](https://github.com/memento-engineering/lenny) | **Leonard** — an agent harness that drives a real Flutter app over the Dart VM service. It taps, types, scrolls, and looks the way a person would, but waits for the frame to settle first. One trustworthy observation per turn. |
| [the_grid](https://github.com/memento-engineering/the_grid) | The orchestrator. A resident station observes a [beads](https://github.com/steveyegge/beads) work graph and reconciles it as a tree — **mount = spawn, unmount = kill**. Each ready bead gets a git worktree, a coding agent, a committee of critics, and a landed PR. |
| [power_station](https://github.com/memento-engineering/power_station) | First-party asset packs for the_grid — the code, dart, federation, and zero-conf domains. Opinions live here, never in the engine. |
| [space_station](https://github.com/memento-engineering/space_station) | memento's own grid instance — the assembled station runner. Downstream stations extend it, never fork it. |

Some numbers from the bench:

- **2,619** beads tracked across six work graphs
- **~91%** completion rate (2,379 closed)
- **~2,600** commits landed, most of them written by agents and gated by other agents
- the whole org is younger than this README's last update — `genesis` and `the_grid` started June 11, the stations on July 1

This replaces the Go-then-Gas-City "factory loop" I was running earlier this year. Same idea, rebuilt Dart-native and reactive: the work graph is *observed* rather than polled, and the running system of agents is a reconciled tree rather than a supervisor loop.

**[Butane](https://github.com/nicholasspencer/butane_flutter) 🔥**

A federated Flutter Bluetooth Low Energy plugin, and the app-shaped half of the bet above. Central *and* peripheral roles on iOS, macOS, Linux (BlueZ over D-Bus), and Android — with a Windows central implementation landing now. The first seven packages went out at **0.1.0** on pub.dev this summer.

The part I actually enjoy: it's verified by cross-device "burns." A grid station leases real hardware, launches a follower app on one machine and a central on another, and Leonard drives both over the VM service through a 15-step scenario — Android ↔ macOS, both directions, screen-recorded. Testing a radio stack against another *real* radio stack, driven by an agent, turns out to catch things a mock never will.

**Local inference on Apple Silicon**

Still running **swift-infer**, a local inference orchestration server in Swift — multiple model backends behind one OpenAI-compatible API, routing each request to whichever node makes sense. It quietly became infrastructure rather than a project: it's one of the backends behind Leonard's model provider, so a good chunk of the agent traffic above never leaves the house. Serving Qwen 3.6 on MLX at last check.

---

*Last updated: August 8, 2026*
