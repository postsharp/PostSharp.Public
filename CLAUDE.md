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
- Release branches are `release/YYYY.N`. Create from `master` if it doesn't exist.
- The tag must be created and pushed before creating the GitHub release. The release targets the branch (`--target release/YYYY.N`).
- When releasing an older version line, use `--latest=false` to avoid overriding the "latest" flag on a newer release. Always check existing releases first.

### CHANGELOG.md
- Maintained on the release branch. Add new entries at the top (below the header).
- Commit and push to the release branch before tagging.

### Release notes content
- Only include **Fixes** — omit enhancements and internal changes as they are not meaningful to customers.
- Use full issue links: `[#N](https://github.com/postsharp/PostSharp.Public/issues/N)`.
- Include a Resources section linking to the milestone.

### Post-release
- Close the milestone.
- Comment on each milestone issue with a link to the release, signed `— Claude`.
