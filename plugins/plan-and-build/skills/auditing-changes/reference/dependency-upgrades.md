# Auditing a dependency upgrade

The diff for a dependency upgrade is the change in the code that will run in production. That code
is the released package for the platform production uses, plus every other package the upgrade
moves in the lockfile. The repository on GitHub and the changelog describe that code, but they
aren't the code.

## Choose the target version

- Read the advisory, such as the GitHub Security Advisory or the project's security notes, for the
  affected versions, the patched versions, and the commits that contain the fix.
- Prefer the smallest step that includes the fix, such as a patch release on the version line the
  application already uses, over the latest release. Every extra release in the jump adds changes
  to audit that the fix doesn't need. Report the version you chose and the alternatives.
- Confirm that the target release contains the fix by finding the fixed code in the released
  package, not only in the changelog.

## Diff the released packages

- Fetch the released package for both versions. For a gem, run `gem fetch <name> -v <version>`
  with `--platform <platform>` when the gem ships builds for each platform, then `gem unpack`.
  Other ecosystems have equivalents, such as `npm pack <name>@<version>` and
  `pip download --no-deps <name>==<version>`.
- Use the platform from the lockfile and production. A precompiled native gem, such as Nokogiri,
  bundles C libraries like libxml2, and changes to those libraries are part of the upgrade.
- Run `diff -ruN` on the two unpacked trees. This diff is what the ledger covers.
- Compare each released package with the source at its tag. A file in the package that is missing
  from the tag, or that differs from it, is a finding until it's explained, because that is where a
  compromised release would show.
- Read the changelog for every release in the jump, and check each entry against the diff, because
  changelogs leave changes out. When you need the reason behind a hunk, such as the issue it fixed,
  ask the researcher subagent.

## Account for everything the upgrade moves

- Run the upgrade in the worktree in its most conservative form, e.g.,
  `bundle update --conservative <name>`, and diff the lockfile. Audit every other package whose
  version changes, or pin it back.
- Check changed requirements, such as the language version, minimum versions of other packages,
  native build dependencies, and new runtime dependencies.
- Check code that runs when the library loads, such as Railties, initializers the library adds, and
  patches it applies to other classes.

## Find where the application depends on the library

The ledger's reachability decisions rest on this list, so build it before you classify the hunks:

- Direct calls, subclasses, included modules, and configuration, such as initializers and YAML
  files.
- Monkey patches, refinements, `prepend`, and code that calls private methods or reads internal
  constants. These break without an error when the internals change, so check each one against the
  new version.
- Other dependencies that call the library. List them with
  `gem dependency <name> --reverse-dependencies` or from the lockfile, because their calls are part
  of the application's behavior.
- Data the old version wrote that the new version reads, such as serialized objects, cache entries,
  cookies, signed or encrypted tokens, and file formats.

## Check the security fix

- Find the fix in the diff, and confirm that it closes the vulnerability the advisory describes,
  including other entry points with the same flaw.
- Determine whether the application was exposed, i.e., whether it reaches the vulnerable code with
  input an attacker controls. Exposure changes how urgent the upgrade is, not how thorough the
  audit has to be.
- When the fix tightens validation or escaping, list the inputs the application sends that the new
  check treats differently, and test each one with data shaped like production data. Security fixes
  cause regressions here most often.
