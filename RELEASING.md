# Releasing

Releases are automated with [release-please](https://github.com/googleapis/release-please).

## Cutting a release

1. Go to **Actions → release-please → Run workflow** and set `bump-type`: `auto` follows conventional commits, `patch`/`minor`/`major` force that bump, `explicit` uses the `release-version` input (a suffix like `0.2.0-beta.1` creates a pre-release).
2. release-please opens a PR titled `release: vX.Y.Z` with the version bump and updated `CHANGELOG.md` — review it.
3. Merge it. CI pushes a signed `vX.Y.Z` tag and creates a **draft** GitHub release.
4. Publish the draft from the Releases page with **Publish this Action to the GitHub Marketplace** checked.
5. Update the `openfga/action-openfga-deploy@vX.Y.Z` pins in `README.md`.

## Version bumps

Bumps follow [conventional commits](https://www.conventionalcommits.org/), with `bump-minor-pre-major` and `bump-patch-for-minor-pre-major` enabled:

| Commit | Before v1.0.0 | v1.0.0 and later |
|---|---|---|
| `feat:` | patch | minor |
| `fix:`, `perf:`, `refactor:`, `revert:`, `docs:` | patch | patch |
| `type!:` or `BREAKING CHANGE:` footer | minor | major |

## Changelog

Built from commit messages since the last release: `feat:` → Added, `fix:` → Fixed, `perf:`/`refactor:` → Changed, `revert:` → Removed, `docs:` → Documentation. `test:`, `ci:` and `chore:` commits are hidden.

If only hidden types landed since the last release, `auto` opens no PR; a forced bump (`patch`/`minor`/`major`/`explicit`) still does — the hidden commits just aren't listed in the changelog. To surface one, title user-facing dependency bumps (e.g. to `action.yml`) as `fix(deps):`, or rewrite them with the override block below.

### Overriding an entry

If a commit message isn't user-facing, add this block to the PR description (before or after merging; re-run the release workflow to pick it up). release-please uses it instead of the commit message:

```
BEGIN_COMMIT_OVERRIDE
feat: user-facing description of the change
END_COMMIT_OVERRIDE
```

### Breaking changes

Mark the type with `!` and add a `BREAKING CHANGE:` footer. PRs are squash-merged and a single-commit PR defaults to its commit title, not the PR title, so make sure the squash commit title has the `!` and add the footer to the squash commit body:

```
feat!: remove the Foo option

BREAKING CHANGE: the Foo option has been removed. Use Bar instead.
```

This gets its own changelog section and triggers a major bump (minor before v1.0.0).
