## Howdy y'all

I'm Nico — Lead Software Engineer at [@Resideo](https://github.com/resideo), based in Austin.

Been building software for over twenty years now. Started with PHP and MySQL back in '05, picked up iOS development early on, and it stuck. Spent time at Match, Shyp, Synack, and American Airlines before landing at Resideo, where I lead mobile engineering. Most of my work these days is in Flutter — and lately, in Dart everywhere else too.

### What I'm tinkering with

**[memento.engineering](https://github.com/memento-engineering)**

Nearly all of my nights-and-weekends work now lives in one place: a small org where the whole toolchain is Dart, top to bottom. Apps are built in Flutter. Leonard drives and debugs the apps. Leonard files bugs as beads into the grid. The grid builds everything.

That's one substrate engine, a debugging harness that drives running programs, and a work-graph orchestrator that spawns coding agents against the ready frontier — all in the same language, so a fix in the substrate shows up in the harness and the orchestrator without a translation layer in between.

| Repo | What it is |
|------|------------|
| [genesis](https://github.com/memento-engineering/genesis) | The substrate. Flutter's element model extracted to pure Dart — a framework-agnostic `Seed`→`Branch` keyed-reconcile engine, plus perception, taxonomy, an A2UI wire, and a terminal render backend. On pub.dev. |
| [leonard](https://github.com/memento-engineering/lenny) | An agent harness that drives a real running program over the Dart VM service. It taps, types, scrolls, and looks the way a person would, but waits for the frame to settle first. One trustworthy observation per turn. A Flutter widget tree was the first target; a tmux server and a native mobile accessibility tree are now the same driver wearing a different perception. |
| [the_grid](https://github.com/memento-engineering/the_grid) | The orchestrator. A resident station observes a [beads](https://github.com/steveyegge/beads) work graph and reconciles it as a tree — **mount = spawn, unmount = kill**. Each ready bead gets a git worktree, a coding agent, a committee of critics, and a landed PR. |
| [power_station](https://github.com/memento-engineering/power_station) | First-party asset packs for the_grid — the code, dart, federation, and zero-conf domains. Opinions live here, never in the engine. |
| [space_station](https://github.com/memento-engineering/space_station) | memento's own grid instance — the assembled station runner. Downstream stations extend it, never fork it. |
| [decisions](https://github.com/memento-engineering/decisions) | A register is a **citation graph with force**, not a document tree: every decision binds the moment it's written, and the consolidated `ADR-000N` document is a generated view over the graph rather than a destination. A [MADR](https://github.com/adr/madr) 4.0 profile that degrades to grep. |

Some numbers from the bench:

- **2,982** beads tracked across seven work graphs, **~88%** closed
- **~3,250** commits landed, most of them written by agents and gated by other agents — **~700 of those in the last month alone**
- **37 packages** on pub.dev — eight `genesis_*`, nine from the_grid, thirteen `leonard_*`, seven Butane
- still a young org — `genesis` and `the_grid` started June 11, the stations July 1

Most of August went into the **trajectory log**. Every session the grid runs now lands in an append-only record — its own database, one table, a single fenced appender — migrated in stages behind a dual-write shadow window so the old read path and the new one could be diffed against each other before the cut. What it buys is `traj committee-report`: fold the verdicts, gates, respec outcomes and token spend across sessions and you get per-lane precision, override rate, respec convergence, and cost. The grid can finally answer whether its own committee of critics is worth what it costs to run — which is the question I've wanted to ask since the first one shipped.

Leonard's decision oracle became pluggable in the same stretch. `leonard_acp` drives any [ACP](https://agentclientprotocol.com)-compatible coding agent as the per-turn brain behind the existing model-provider seam, so a live app can be steered by a hosted model, a local one, or whatever coding agent you already have open. Meanwhile the stations grew real seats — per-seat GitHub App clients, named agent environments picked in code — and the last week of the month went to `decisions`, because every repo had grown its own ADR pile and I'd rather have a graph than a filing cabinet.

This whole setup replaces the Go-then-Gas-City "factory loop" I was running earlier this year. Same idea, rebuilt Dart-native and reactive: the work graph is *observed* rather than polled, and the running system of agents is a reconciled tree rather than a supervisor loop.

**[Butane](https://github.com/nicholasspencer/butane_flutter) 🔥**

A federated Flutter Bluetooth Low Energy plugin — and the app end of everything above. Central *and* peripheral roles on iOS, macOS, Linux (BlueZ over D-Bus), and Android. Seven packages are out at **0.1.0** on pub.dev; the Windows central implementation is still on the bench, unpublished, waiting for me to want it badly enough.

The part I actually enjoy: it's verified by cross-device "burns." A grid station leases real hardware, launches a follower app on one machine and a central on another, and Leonard drives both over the VM service through a 15-step scenario — Android ↔ macOS, both directions, screen-recorded. Testing a radio stack against another *real* radio stack, driven by an agent, turns out to catch things a mock never will.

**Local inference on Apple Silicon**

Still running **swift-infer**, a local inference orchestration server in Swift — multiple model backends behind one OpenAI-compatible API, routing each request to whichever node makes sense. It quietly became infrastructure rather than a project: it's one of the backends behind Leonard's model provider, so a good chunk of the agent traffic above never leaves the house. The catalog's on Qwen 3.8 and Gemma 4 these days.

---

*Last updated: September 2, 2026*
