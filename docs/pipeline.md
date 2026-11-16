# The pipeline

How a request becomes a released feature. Every step below is carried out by an AI agent unless it says "I".

```
 Idea ──► Requirements ──► Design ──► Build ──► Push ──► Live ──► Review
 (I write)  (questions)   (mockups)  ("build")  ("push")  (auto-update)
```

## 1. Idea

I add a row to the tracker in plain language, for example "show the time left on the shipyard queue header". A requirements agent gives it an ID with a prefix for its feature area (such as `SH-030` for Shipyard) and routes it to the agent that owns that area. The ID follows the item all the way to the release notes.

## 2. Requirements

The owner reads the request, checks the style guide and its own notes, and asks me only what is really open. Every question comes with a recommended default, so I can answer a batch with the single word "defaults".

## 3. Design

For anything visual, the owner produces a mockup at my real screen size before coding. If the work touches a shared area (table look, fleet dialogs, data loading or badges), the owner sends the shared owner a short note and gets back "fits", "use helper X" or "conflicts with Y". This is a check-in, not a sign-off, so it never blocks.

The default is minimal: build the feature asked for, plus only what is needed to use it.

## 4. Build

Nothing is coded until I say **build**. Then each owner with an approved design:

1. Codes its items, editing only its own modules.
2. Runs the syntax check.
3. Runs the tests related to what it changed. A helper script finds them from the module names and requirement IDs.
4. Writes or updates a browser test for the new behavior.
5. Reports "ready" to the coordinator.

## 5. Push

Nothing is released until I say **push**. The coordinator then runs a checklist: it reads every active thread, sorts built items into "this push" and "later", and waits for anything still being coded so that a release is never partial. It hands the Core agent an exact list.

Core is the only agent that builds and publishes. One command does the following, and stops at the first failure:

| Step | What it does |
|---|---|
| Join | Concatenates the numbered modules in order, each feature module in its own scope |
| Stamp | Writes the version into the script header. A version number can never be reused |
| Syntax check | Parses the whole script |
| Style lint | Fails on new breaks of the style guide that a machine can detect |
| Safety lint | Fails on a code pattern that once caused a serious bug |
| Strip comments | Removes every comment from the published copy, then parses it again |
| Test | Runs all 288 browser tests against the finished build. Any failure deletes the build |
| Publish | Copies the build to the distribution repository with a version file and release notes |

Release notes are written for players: short bullets grouped by area, with requirement IDs so any line can be traced back.

## 6. Live

Installed copies update themselves from the distribution repository, and the script shows a small bar when a newer version exists. The coordinator tells every thread whose work shipped. Each marks its tracker rows Delivered and tells me in one line.

A QA agent then opens the live game in read-only mode, confirms the new version is running, and walks a checklist for the pages that changed. It never clicks anything that changes game state. Bugs it finds go to the owner of the affected module as new tracker rows.

## 7. Review

- Each owner reviews what shipped in its area on a schedule and files recommendations as ideas. Reviews never apply fixes.
- Six refactor signals (repeat bugs, rework, duplication, module growth, stale switches, slow tests) trigger a refactor proposal with evidence.
- A weekly retrospective looks at trends across the whole project: rework, approvals per item, build count.

Nothing from a review changes the code or the process until I approve it.

## The test suite

Each test is a small script that launches a headless browser, serves a mock of one game page, injects the built userscript, and asserts on the result. Most tests also call a shared layout check that fails on overlapping elements, wrapped table rows and misaligned numbers.

Things that kept the suite fast and trustworthy as it grew from about 120 tests to 288:

- **Parallel runs on one shared browser.** A full run dropped from about 10 minutes to under 5.
- **Slowest first.** Tests start in order of their last run time, so one slow test never holds up the end.
- **A pinned clock.** The run's time zone is chosen so that it is always about noon, after a test failed once near midnight.
- **Related tests only during development.** Only the release runs the full suite. This was the single biggest cost saving in the project.
- **Flakiness is tracked.** A failed test gets one serial retry to separate timing from real failures. Every retry is logged with the release, and the test is fixed.

## Safety rules

- Agents have read-only access to the live game. Only I play.
- A single word from me stops all browser activity by every agent at once, for moments when the game needs my full attention.
- The live script is never overwritten in place. A rollback is the old code published under a new, higher version.
