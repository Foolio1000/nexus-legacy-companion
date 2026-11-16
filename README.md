# Nexus Legacy Companion

A browser userscript for the space strategy game Nexus Legacy, and a case study in running a software project with a team of AI agents.

I play as **Foolio**. I wanted the game's interface to do more of the bookkeeping for me, so I started specifying improvements and directing Claude agents to build them. Three weeks later the result was a 34,000-line script, shipped in 127 releases, with a test suite, a style guide and a release process that takes two words from me.

This repository describes the product and how it was built. The source code lives in a private repository.

| | |
|---|---|
| **Releases** | 127 versions in 22 days (Oct 25 to Nov 15, 2026) |
| **Tracked requirements** | About 680, each with an ID that follows it from idea to release note |
| **Source** | About 34,000 lines of JavaScript in 110 modules, built into one userscript |
| **Automated tests** | 288 browser tests, all of which must pass before a release is published |
| **Team** | One person as product owner, about 20 AI agents with standing roles |

## What the script does

Nexus Legacy is a browser game about running a space empire: planets, moons, fleets, research, trade and combat. A mid-game account means dozens of timers and hundreds of small decisions a day, spread over many screens.

The script runs inside the game page and extends the game's own interface rather than replacing it. It adds tables, buttons, badges and dialogs that look like they belong there, and it can be switched on or off feature by feature.

**Know what needs attention.** Every item in the game's menu gets a single badge that counts the things waiting on you: an idle shipyard, a full storage, a fleet that needs a decision. Hovering lists each one, and clicking takes you to the exact planet and tab. Routine notifications are silenced, so a badge always means something.

**See everything in one table.** Card-heavy pages (buildings, research, shipyard, fleets, moons, spy reports) gain a Table View with one shared standard: the same fonts, alignment, sorting and filters everywhere, remembered per table.

**Launch fleets in fewer clicks.** A shared fleet dialog fills in sensible defaults, shows travel time, fuel and arrival, and warns before a costly mistake. One click mode sends routine missions with the defaults and still opens the dialog if anything looks wrong.

**Move resources without the arithmetic.** A Transfer tab works out which planets, moons and outposts are over or under their thresholds and proposes the shipments. Saved routes and a "bring missing resources" button cover the common cases.

**Plan across the whole empire.** A Command Center page shows fleets, resources, research and build queues for every planet in one place, with return-on-investment figures for buildings and research.

**Respond to attacks quickly.** When an attack is incoming, a banner shows what is at risk and offers the two or three useful responses. Station defense and offense pages track every contested station, with the reinforcement numbers already worked out.

**Read intel at a glance.** Spy reports become sortable tables with a change-since-last-report view, and a target list keeps notes, timers and the latest report for each target.

**Stay current.** The script updates itself, shows a small bar when a new version is available, and links to the release notes.

## How it is built

**One script from many modules.** Each feature area is its own numbered module. A build script joins them in order, wraps each in its own scope so names cannot collide, stamps the version, and checks the result. A small core gives every module the same tools: a feature registry with an on/off switch and error isolation per feature, a watcher that re-applies changes when the game redraws, shared styles, settings, and a fleet dialog.

**A light footprint on the game.** The script listens to the data the game page already loads and shares it between features through a cache, so most features need no requests of their own. The few that do are paced, never retry, and back off when the server asks. Every repeating timer in the script is listed in one table with the requirement that approved it.

**Tests that run the real build.** Each of the 288 tests loads the built script into a headless browser against a mock of the game page, at the screen size I actually play at, and checks behavior and layout. A style check fails the build on any overlapping elements, misaligned numbers or rows that wrap. Two lint passes enforce rules that came out of real bugs. If any test fails, the build is deleted.

**Continuous delivery.** A release is one command: build, test, publish, changelog. Installed copies update themselves.

More detail: [the pipeline](docs/pipeline.md).

## How the work is organized

I act as product owner. I write requirements, answer design questions, and decide when to build and when to release. The agents do the rest, and the interesting part of the project turned out to be the working model that lets about 20 of them cooperate on one codebase without stepping on each other.

- **A coordinator** takes my requests, routes each to the right owner, and reports status as a single table.
- **Standing owners** each own a set of modules and are the only ones who edit them. Feature owners cover areas of the game. Shared owners cover things every feature uses: look and feel, fleet dialogs, data and refresh, and badges.
- **One Core agent** is the only one that builds and publishes, so there is exactly one path to production.
- **A tracker** holds every requirement and its status. Nothing is built without an approved row.
- **A style guide** records every recurring design decision I make, so I am asked once.
- **Gates** separate the phases. Nothing is coded until I say "build" and nothing ships until I say "push".
- **Review loops** keep it healthy: scheduled code reviews by each owner, a weekly retrospective, a daily memory check, and a periodic handover of each role to a fresh agent.

More detail: [the working model](docs/working-model.md).

## What I learned

- **Ownership beats coordination.** Early on, a new agent was opened for each task, and their work overlapped on shared files. Giving every module exactly one standing owner removed most of the rework.
- **Write decisions down once.** Each time I answered the same design question twice, the answer went into the style guide. The agents stopped asking and the interface became consistent.
- **Short keywords make good gates.** "Build" and "push" each mean one precise thing. That let me approve work in seconds without reading a paragraph first.
- **Tests are what make speed safe.** Several releases a day was only possible because every one ran the full suite against the real build. Flaky tests were logged at every release and fixed, not ignored.
- **Turn bugs into rules.** The worst bug mixed up data between planets. The fix shipped with a lint check that now fails any build that could repeat it.
- **Process needs maintenance too.** The working model changed almost weekly. A standing retrospective, with nothing adopted until I approve it, kept those changes deliberate.

## Release history

[Release highlights](RELEASES.md) summarizes the 127 versions by milestone.

## Notes

This is a personal fan project. It is not affiliated with or endorsed by the makers of Nexus Legacy. Details about other players, game internals and source code are intentionally left out of this repository.
