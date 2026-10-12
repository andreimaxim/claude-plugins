---
name: upgrade-gem
description: Upgrade a gem in a Ruby or Rails application to a requested version. Plans the upgrade with the gem-upgrade-planner subagent, waits for user approval, then implements and verifies the approved changes.
argument-hint: <gem> to <version>
---

Upgrade a gem in this application: $ARGUMENTS

1. Identify the gem and the requested version. Ask if either is missing or unclear.
2. Find the installed version from the lockfile or Bundler. Resolve the requested version to an exact released version; for a partial version such as `8.1`, use the latest release in that series. If the application is already at or past that version, say so and stop.
3. Delegate planning to the `maintain-rails:gem-upgrade-planner` subagent. Give it the gem, `currentVersion`, and `requestedVersion` (the exact version).
4. Present the plan and wait for the user's approval. Do not change the application before then.
5. Implement the approved changes, then run the plan's validation steps and report the results. If the work grows materially beyond the approved plan, stop and get approval for the expanded scope first.
