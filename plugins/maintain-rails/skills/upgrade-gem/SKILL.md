---
name: upgrade-gem
description: Upgrades gems in Ruby or Rails applications when the user asks to update a dependency.
argument-hint: <gem> to <version>
---

# Upgrading a gem

Upgrade the requested gem and make the application changes needed for compatibility.
Use `$ARGUMENTS`, when supplied, to identify the gem and requested version.

## Upgrade scope

Ask if the gem or requested version is missing or unclear.

Use `bundle info <gem>` to find the installed version. If the bundle is unavailable,
read the version from `Gemfile.lock`.

Use `gem list --remote --all --exact <gem>` to look up available releases. Resolve
the requested version to an exact release. For a partial version such as `8.1`, use
the latest release in that series.

If the installed version is already at or beyond the target, report that and stop.

Check whether the target satisfies the gem's version requirement in the Gemfile.
If it does not, show the current requirement and the proposed replacement, and ask
the user to explicitly approve that change before modifying the Gemfile.

## Application-specific planning

Delegate the application-specific upgrade plan to the
`maintain-rails:gem-upgrade-planner` subagent. Provide the gem and the exact versions
as `currentVersion` and `requestedVersion`.

## Approval and validation

Present the plan and wait for the user's approval before changing the application.
Keep changes within the approved scope. Ask for approval again if the work expands
materially beyond the plan.

Run the plan's validation checks and report the results.
