# Release highlights

The project began at version 0.1 in early October 2026, about six weeks before this page was written. It started as a handful of small scripts and was rebuilt as one unified script, version 0.5, on October 25. That script shipped 127 versions by November 15. This page summarizes the work by milestone. The full notes are written for players and list every change with its requirement ID.

## The first scripts (v0.1 to v0.4)

*Early October to Oct 24*

The detailed release notes begin with v0.5, so this phase is summarized only in outline.

- **Five single-purpose scripts.** A general interface cleanup, a task dashboard, a PVE dashboard, a PVP helper and an empire overview, each built and installed separately.
- **What they proved.** The ideas were worth having, and the approach of extending the game's own pages worked better than separate dashboards.
- **Why they were replaced.** As they grew they began changing the same parts of the page and loading the same data twice. Every new feature risked breaking another script.

## One unified script (v0.5.0 to v0.5.60)

*Oct 25 to Nov 1*

- **Unified script.** A core with a feature registry, a settings screen and a per-feature on/off switch replaced a set of separate scripts that had started to collide.
- **A cleaner frame.** Rebuilt left menu, top banner and options panel, in the game's own look.
- **Planet pages.** Compact list views, filters and hide buttons for Buildings, Research and Shipyard.
- **Fleet and galaxy.** An Evacuate action that moves ships and cargo out of harm's way, and a systems table for the galaxy map.
- **Command Center.** One page for the whole empire: to-do list, buildings, research and fleet composition.
- **Retiring the old scripts.** Each older script was replaced feature by feature, with a logged decision for every overlap.

## Delivery and scale (v0.5.61 to v0.5.100)

*Nov 2 to Nov 6*

- **Continuous delivery (v0.5.68).** Builds publish themselves and installed copies auto-update. An in-game bar announces new versions with a link to the notes.
- **Ready to share (v0.5.80).** Hard-coded numbers became settings, account-specific defaults were removed, and comments are stripped from the published script.
- **Transfer tab.** Works out what each planet, moon and outpost needs or can spare, with saved routes.
- **Badges that mean something.** A badge registry gives every menu item one count, a hover that lists the causes, and a click that goes to the right place. Badges clear on the action that resolves them.
- **Attack response.** An attack banner shows what is at risk. A Fire Sale dialog and a station defense page turn a scramble into a few clicks.
- **Wormhole runs.** Active runs are tracked, with a badge when one is waiting for a decision.
- **Spy reports as tables.** Sortable reports with previous and next, plus a quick-spy launcher.
- **Engineering.** The full test suite became a required build step. Style checks joined the build. The largest module was split into smaller files. An error log was added to the settings screen.

## Consolidation (v0.5.101 to v0.5.116)

*Nov 6 to Nov 12*

- **Player-friendly release notes (v0.5.101).** Short grouped summaries replaced one line per requirement.
- **PVE moves into Fleet (v0.5.101).** Pirates, Surveys, Debris, Mining and Ruins became Fleet tabs with Table View, and the old standalone dashboard was retired.
- **One click mode.** Routine missions launch with defaults. The dialog still opens if anything is off, and the riskiest mission types always open it.
- **Planet safety fix (v0.5.102).** A bug that could mix data between planets was fixed at the root, with a lint check added to the build so it cannot return.
- **Station defense, rounds 2 to 6.** Sections for at-risk, covered and resolved stations, reinforcement numbers from a shared combat model, recall buttons, and a matching page for our own attacks.
- **Quieter game.** Routine messages are marked read silently. Each badge type has its own switch.
- **Moons.** A rebuilt scans table, thresholds for what each moon should keep, and one-click collection.
- **Faster tests.** The suite was reorganized to run in parallel on a shared browser, more than halving its run time while it kept growing.

## Polish and standards (v0.5.117 to v0.5.126)

*Nov 12 to Nov 15*

- **A target list.** A dedicated page for tracking targets, with filters per section, timed hides, notes, and the latest report one click away.
- **Own attack and spy dialogs.** Nearest source planet, fuel, travel time and arrival, all filled in.
- **Combat simulator fill.** The game's simulator fills itself from a spy report and your own fleet.
- **One table standard (v0.5.124).** Every table in the script follows the same header, body, sorting and filter rules, with checks in the test suite.
- **One clock.** Times everywhere read in UTC, replacing a mix of local and server times.
- **Market.** An Open Orders tab shows every open order on every hub in one read.
- **Shared fleet availability (v0.5.126).** One event-driven store replaced several timers that each counted free fleet slots their own way.
- **Second server.** Features were adapted to work on a second game server with different internals.

## By the numbers

| | |
|---|---|
| Unified script versions | 127 in 22 days |
| Requirements delivered | About 680 |
| Busiest release | 49 requirements (v0.5.101) |
| Tests on Nov 6 | About 120 |
| Tests on Nov 15 | 288 |
