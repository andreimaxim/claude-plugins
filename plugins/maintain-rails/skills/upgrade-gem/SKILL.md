---
name: upgrade-gem
description: Plans gem upgrades in Ruby or Rails applications. Use when the user asks to upgrade, update, or bump a gem.
argument-hint: <gem> to <version>
---

# Planning a gem upgrade

Identify the gem and requested version from the request or `$ARGUMENTS`. Ask if
either is missing or unclear.

Delegate to `maintain-rails:gem-upgrade-planner` with the gem and exact
`currentVersion` and `requestedVersion`. Return its plan without implementing the
upgrade or modifying the working application.
