# The working model

How one person and about 20 AI agents share a codebase. This model was not designed up front. It grew out of problems, and each rule below exists because something went wrong without it.

## Roles

```
                        ┌──────────────┐
            me  ◄─────► │ Coordinator  │
                        └──────┬───────┘
        ┌──────────────┬───────┴──────┬───────────────┐
  ┌─────▼─────┐  ┌─────▼─────┐  ┌─────▼─────┐  ┌──────▼─────┐
  │  Feature  │  │  Shared   │  │   Core    │  │  Service   │
  │  owners   │  │  owners   │  │ build and │  │   owners   │
  │           │  │           │  │  publish  │  │            │
  └───────────┘  └───────────┘  └───────────┘  └────────────┘
```

**Coordinator.** My single point of contact. It routes each request to the right owner, relays my "build" and "push", and answers "status" with one table: item, owner, stage, and exactly what is waiting on me.

**Feature owners.** Eight agents, each owning an area of the game end to end: its tracker rows, modules, notes and tests. Examples are Planets, Fleet and Logistics, and Moons and Outposts.

**Shared owners.** Five agents that own what every feature uses: look and feel (including the style guide), fleet dialogs, data and refresh, badges and notifications, and fleet availability.

**Core.** Owns the core modules and the build and publish scripts. It is the only agent that builds, runs the full test suite, and publishes.

**Service owners.** Requirements (tracker rows and IDs), QA (read-only checks of the live game), maintenance, a weekly retrospective, and a daily memory health check.

**One-off agents.** Opened for a single analysis or question and closed when it is delivered.

## Rules that make it work

**Every module has exactly one owner.** Only the owner edits it. Work that needs another owner's file goes to that owner. A roster lists who owns what, and it is the only place that information lives.

**One path to production.** Feature owners never build a release. They report "ready", and Core builds everything together. With one script, that means one build at a time and no merge surprises.

**Two gates, two words.** "Build" means code the approved designs. "Push" means release what is built. Neither implies the other, and an earlier approval never carries over to later work.

**Questions come with defaults.** An agent that needs a decision proposes one. "Defaults" accepts them all. Where an answer can be inferred from an earlier decision, the agent takes it and says so.

**Status lives in the tracker.** Not in agent memory and not in chat. Any agent can be replaced without losing track of the work.

**Decisions live in documents.** There is a specific home for each kind of knowledge:

| Kind of knowledge | Where it lives |
|---|---|
| How the project runs | Project instructions, read by every agent on every turn |
| How to do a task | Skills, loaded when the task comes up |
| Recurring design decisions | The style guide |
| How the game behaves | One game knowledge document, curated by one owner |
| What each module does | A notes file per module, kept by its owner |
| Lessons from mistakes | A learnings file per owner |
| Lasting preferences | A small shared memory with an index |

**Agents are replaceable.** Each owner keeps its files current enough that a fresh agent can take over. A periodic rollover does exactly that: it harvests lessons, prunes memory, and restarts each role with a clean context.

## How it changed

The project is about six weeks old. The first weeks produced separate single-purpose scripts, and most of the model below took shape after the move to one script in late October.

| When | Problem | Change |
|---|---|---|
| Late Oct | Several separate scripts collided on the same page | One unified script built from modules, with a single Core builder |
| Late Oct | Layouts looked right in tests and wrong on my screen | Every test runs at my real screen size and fails on overlap |
| Early Nov | Installing each build by hand was slow | Continuous delivery with self-updating installs |
| Early Nov | The same design questions kept coming back | A style guide that agents apply without asking |
| Early Nov | Process changes piled up informally | A weekly retrospective. Nothing is adopted until I approve it |
| Early Nov | Releases shipped partial work | A pre-push checklist and a rule that a push waits for work in progress |
| Early Nov | A bug mixed data between planets | One shared source for "which planet is on screen", enforced by a lint |
| Mid Nov | Every agent ran the full test suite | Related tests during development, full suite only at release |
| Mid Nov | Task-based agents overlapped on shared files | Standing owners with a roster, plus check-ins with shared owners |

## What I do

- Write requirements in plain language.
- Answer design questions, usually by accepting a default.
- Say "build" and "push".
- Use the result in the game and report what feels wrong.
- Approve or decline changes to the process itself.

Most of that happens a few minutes at a time.
