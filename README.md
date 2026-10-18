## Howdy y'all

I'm Nico — Lead Software Engineer at [@Resideo](https://github.com/resideo), based in Austin.

Been building software for over twenty years now. Started with PHP and MySQL back in '05, picked up iOS development early on, and it stuck. Spent time at Match, Shyp, Synack, and American Airlines before landing at Resideo, where I lead mobile engineering. Most of my work these days is in Flutter — and lately, in Dart everywhere else too.

### What I'm tinkering with

**[memento.engineering](https://github.com/memento-engineering)**

Nearly all of my nights-and-weekends work now lives in one place: a small org where the whole toolchain is Dart, top to bottom. Apps are built in Flutter. Leonard drives and debugs the apps. Leonard files bugs as beads into the grid. The grid builds everything.

That's one substrate engine, a debugging harness that drives running programs, and a work-graph orchestrator that spawns coding agents against the ready frontier — all in the same language, so a fix in the substrate shows up in the harness and the orchestrator without a translation layer in between.

| Repo | What it is |
|------|------------|
| [genesis](https://github.com/memento-engineering/genesis) | The substrate. Flutter's element model extracted to pure Dart — a framework-agnostic, bare-VM `Component` → `Element` keyed-reconcile engine. It borrows Flutter's vocabulary but not its inheritance: an `Element` never *is* its `BuildContext`. Plus perception, taxonomy, the A2UI wire, a terminal render backend, and analyzer lints that keep a build declarative. On pub.dev. |
| [leonard](https://github.com/memento-engineering/lenny) | An agent harness that drives a real running program over the Dart VM service. It taps, types, scrolls, and looks the way a person would, but waits for the frame to settle first. One trustworthy observation per turn. A Flutter widget tree was the first target; a tmux server, a native mobile accessibility tree, and now a macOS app are the same driver wearing a different perception. |
| grid | The orchestrator, rewritten. A `genesis_tree` component tree reconciled against two scopes — the work in [beads](https://github.com/gastownhall/beads), the execution in the grid's own authority — into supervised agent processes that build, review, and land the work. Private while it finds its feet. |
| [the_grid](https://github.com/memento-engineering/the_grid) | The proof. The first Dart-native orchestrator: a resident station observes a beads work graph and reconciles it as a tree — **mount = spawn, unmount = kill** — giving each ready bead a git worktree, a coding agent, a committee of critics, and a landed PR. It ran the org from July to October. |
| [power_station](https://github.com/memento-engineering/power_station) · [space_station](https://github.com/memento-engineering/space_station) | the_grid's asset packs (code, dart, federation, GitHub, zero-conf) and memento's own assembled station. Opinions live in the packs, never in the engine. |
| [butcher](https://github.com/memento-engineering/butcher) | Mutation testing for Dart: rewrite the source into mutants, run the tests that cover each one, score which ones the suite kills. A fork of Ricardo Boss's [radioactive_dart](https://github.com/ricardoboss/radioactive_dart), split into three packages on pub.dev. Leonard's own mutation lane runs on it. |
| [decisions](https://github.com/memento-engineering/decisions) | A register is a **citation graph with force**, not a document tree: every decision binds the moment it's written, and the consolidated document is a generated view over the graph. A [MADR](https://github.com/adr/madr) 4.0 profile that degrades to grep — and now the only decision system in every repo. |

Some numbers from the bench:

- **3,790** beads tracked across ten work graphs, **~91%** closed
- **~4,600** commits landed, most of them written by agents and gated by other agents — **~1,800 since the start of September**
- **48 packages** on pub.dev — nine `genesis_*`, thirteen `leonard_*`, ten from the_grid, six power_station asset packs, three from butcher, seven Butane
- still a young org — `genesis` and `the_grid` started June 11; `grid` started October 4

The last six weeks were mostly **a rewrite**. September went into hardening the_grid: about a thousand commits across the engine and its asset packs, plus the later stages of moving the trajectory log out of beads and into its own store. In early October I stopped and had the grid reviewed: six read-only surveys across every repo it touched. The good news was that the bet held up. A Dart-native station really can reconcile a work graph into supervised agents that build, review, and land beads, and it had been doing that across thirteen stores since July. The bad news was the bill: about 228k lines across four repos and 27 packages, for a proof of concept with one operator. Most of that cost came from a few early choices. Orchestration state lived *as beads*, an idea borrowed from Gas City, and was then migrated out of beads in place. Imperative code had grown inside a declarative tree. And somewhere between a sixth and a quarter of every repo's commits since September went to a multi-repo release train that no outside consumer needed yet.

So `grid` starts over on what the first one proved. Work and execution are two separate scopes. The tree is the context, and effects are its leaves. A guard is either loud or gone. One repo, one workspace, and nothing published until a second org needs it. The first milestone is done: one composed station, dry by default, that can go live, run agents under a reaper in per-bead worktrees, and land a bead on a pushed branch. Next is grid building its own work. The first bead is already filed and waiting for my approval.

Leonard had a quieter stretch, and that's a good sign. It picked up a macOS native backend driven through Appium mac2, per-run reporting of token use, cost, and model-call latency, and a stable release of all thirteen `leonard_*` packages on the stable Genesis line. Genesis gave up its botanical names in the same stretch. `Seed` and `Branch` became `Component` and `Element`, the vocabulary every Flutter developer already reads fluently, and that went stable in `genesis_tree` 0.6.0 alongside a set of tree-shape lints. The org also finished one cleanup: the old per-repo decision documents are retired, and `decisions` is now the only register.

This whole setup replaces the Go-then-Gas-City "factory loop" I was running earlier this year. Same idea, rebuilt Dart-native and reactive: the work graph is *observed* rather than polled, and the running system of agents is a reconciled tree rather than a supervisor loop.

**[Butane](https://github.com/nicholasspencer/butane_flutter) 🔥**

A federated Flutter Bluetooth Low Energy plugin — and the app end of everything above. Central *and* peripheral roles on iOS, macOS, Linux (BlueZ over D-Bus), and Android. All seven packages moved to **0.2.0** on pub.dev in October — mostly the bugs you only find on real radios: Android connect/disconnect edge cases, 16- and 32-bit UUIDs, CoreBluetooth descriptor types, peripherals that stopped re-reporting on rescan. The Windows central implementation is still on the bench, unpublished, waiting for me to want it badly enough.

The part I actually enjoy: it's verified by cross-device "burns." A grid station leases real hardware, launches a follower app on one machine and a central on another, and Leonard drives both over the VM service through a 15-step scenario — Android ↔ macOS, both directions, screen-recorded. A cabled iPhone now gets its VM service forwarded over USB, so a burn no longer depends on macOS granting Local Network access to the right binary. Testing a radio stack against another *real* radio stack, driven by an agent, turns out to catch things a mock never will.

**Local inference on Apple Silicon**

Still running **swift-infer**, a local inference orchestration server in Swift — multiple model backends behind one OpenAI-compatible API, routing each request to whichever node makes sense. It quietly became infrastructure rather than a project: it's one of the backends behind Leonard's model provider, so a good chunk of the agent traffic above never leaves the house. The catalog's on Qwen 3.8 and Gemma 4 these days.

---

*Last updated: October 17, 2026*
