# PostSharp.Public

Public issue tracker and GitHub release hub for [PostSharp](https://www.postsharp.net/), a closed-source .NET IL-weaving compiler and aspect-oriented framework.

This repository contains no source code. It is used for:
- Public issue tracking (bugs, feature requests)
- GitHub Releases (changelogs, release tags)

## Versioning Scheme

PostSharp uses calendar-based versioning: `YYYY.N.BB[-maturity]`

- `YYYY` — release year (e.g., `2024`)
- `N` — major release within the year (e.g., `0`)
- `BB` — build/patch number (e.g., `22`)
- Optional maturity suffix: `-preview`, `-rc`

Examples: `2024.0.22`, `2025.1.3-preview`

## Branch Conventions

- `master` — default branch
- `release/YYYY.N` — long-lived release branch for each major version (e.g., `release/2024.0`)

Release branches are created from `master` when the first release for a major version is published.

## Milestone Conventions

Each release corresponds to a GitHub milestone named `vYYYY.N.BB` (e.g., `v2024.0.22`). Milestones collect the issues included in that release.

## Creating a GitHub Release

Use the `eng:prepare-release` skill (`/eng:prepare-release vYYYY.N.BB`). The skill is designed for Metalama but applies here with the following PostSharp-specific differences:

### Repository differences
- All issues, milestones, and releases live in `postsharp/PostSharp.Public` (this repo). There is no separate source repo — PostSharp source is closed.
- There is no Metalama.Compiler or Metalama.Premium equivalent to check.
- There is no GitHub project board — skip project status checks and "Done" status updates.

### Tag and branch conventions
- Tags are named `vYYYY.N.BB` (e.g., `v2024.0.22`), not `release/YYYY.N.BB`.
- The release title is `PostSharp YYYY.N.BB` — the version without the `v` prefix, not the tag name. This matches the convention of the `eng:prepare-release` skill. Every release published before 2026-08-05 was titled after its tag and has since been renamed, so the whole page follows this rule; a title is editable in place with `gh release edit <tag> --title`, which does not disturb the publication date.
- Release branches are `release/YYYY.N`. Create from `master` if it doesn't exist.
- The tag must be created and pushed before creating the GitHub release. The release targets the anchor commit (`--target <anchor-sha>`), never a branch — see "Release ordering" below.
- The GitHub "latest" release must always point to the highest version number. When creating a release for a version that is not the highest, use `--latest=false`. Always check existing releases first.

### Release ordering: anchor commits

The releases page **sorts by `created_at` but displays `published_at`**. These are not the same
date. `published_at` is the moment the release is created through the API; `created_at` is the
committer date of the commit the tag points at, captured when the release is created. Because
this repository holds no source code, its few real commits are old and unrelated to any release,
so targeting a branch stamps a brand-new release with a stale `created_at` and sinks it down the
page while still showing today's date.

Every release therefore gets its own **anchor commit** on `master`, dated at publication time.
`RELEASES.md` on `master` is the index of those commits; the commits carry no other content —
their only purpose is to date the tag.

```bash
git clone https://github.com/postsharp/PostSharp.Public.git && cd PostSharp.Public
NOW=$(date -u +%FT%TZ)
printf -- '- %s (%s)\n' "vYYYY.N.BB" "${NOW%T*}" >> RELEASES.md
git add RELEASES.md
GIT_AUTHOR_DATE="$NOW" GIT_COMMITTER_DATE="$NOW" git commit -m "Release vYYYY.N.BB"
git push origin master
ANCHOR=$(git rev-parse HEAD)

git tag "vYYYY.N.BB" "$ANCHOR" && git push origin "refs/tags/vYYYY.N.BB"
gh release create "vYYYY.N.BB" --target "$ANCHOR" --title "vYYYY.N.BB" --notes-file notes.md
```

Backfilling a release for an already-published build works the same way, except the anchor
commit is dated when that build actually shipped (check nuget.org, not the tags), so it lands in
the right place in the order.

#### Never recreate an older release

`published_at` cannot be set through the API — it is always the moment of creation. Deleting and
recreating a release resets the date shown on the page to today and permanently loses the real
publication date. A release may only be recreated **on the day it was published**. For anything
older, accept the wrong ordering: it is cosmetic, whereas a falsified publication date is not.

### CHANGELOG.md
- Maintained on the release branch. Add new entries at the top (below the header).
- Commit and push to the release branch before tagging.

### Release notes content

Always open with the "based on" intro linking the previous release, so the releases form a chain:

```
PostSharp 2026.0.9 is based on [v2026.0.8](https://github.com/postsharp/PostSharp.Public/releases/tag/v2026.0.8), plus the following changes.

### Fixes
- [#15](https://github.com/postsharp/PostSharp.Public/issues/15) Description

### Resources
- [Milestone](https://github.com/postsharp/PostSharp.Public/milestone/5?closed=1)
```

- When a release forward-ports another version line, name both bases: `is based on [vA] and [vB]`, and summarize the upstream release in a sentence rather than repeating its issues.
- Sections are `Breaking Changes`, `New`, `Enhancements`, `Fixes`, in that order; include only the non-empty ones. Most releases are `Fixes` only — omit internal changes that are not meaningful to customers.
- Use full issue links: `[#N](https://github.com/postsharp/PostSharp.Public/issues/N)`.
- Include a Resources section linking to the milestone.

### Post-release
- Close the milestone, and any stale milestone for the same version on `postsharp/PostSharp`.
- Comment on each milestone issue with a link to the release, signed `— Claude`.
