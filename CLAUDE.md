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

### Prerequisites
- `gh` CLI authenticated with write access to `postsharp/PostSharp.Public`
- All issues for the milestone must be closed

### Process

1. **Fetch milestone issues**
   ```bash
   # Find the milestone number
   gh api repos/postsharp/PostSharp.Public/milestones --jq '.[] | select(.title=="vYYYY.N.BB")'

   # List issues in the milestone
   gh api "repos/postsharp/PostSharp.Public/issues?milestone=NUMBER&state=closed&per_page=100"
   ```

2. **Ensure the release branch exists**
   ```bash
   # Check if release/YYYY.N exists on the remote
   git ls-remote --heads origin release/YYYY.N

   # If not, create it from master
   git checkout master && git pull
   git checkout -b release/YYYY.N
   git push -u origin release/YYYY.N
   ```

3. **Update CHANGELOG.md**

   Add a new section at the top of CHANGELOG.md (create the file if it doesn't exist). Format:

   ```markdown
   ## [YYYY.N.BB] - YYYY-MM-DD

   ### Bug Fixes
   - Description of bug fix (#issue)

   ### Enhancements
   - Description of enhancement (#issue)
   ```

   Commit and push to the release branch.

4. **Create the GitHub release**
   ```bash
   gh release create vYYYY.N.BB --target release/YYYY.N --title "PostSharp vYYYY.N.BB" --notes "RELEASE_NOTES_HERE"
   ```

   Release notes body format:
   ```markdown
   ## Bug Fixes
   - Description (#issue)

   ## Enhancements
   - Description (#issue)
   ```

5. **Close the milestone**
   ```bash
   gh api repos/postsharp/PostSharp.Public/milestones/NUMBER -X PATCH -f state=closed
   ```
