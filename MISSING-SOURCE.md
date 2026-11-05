# Missing source repository: `swiggy-private/zone-rca-degrowing`

The CodeBuild request (Phoenix `phoenix-modernizer`, request ID
`97422ae3-aca6-43a9-80a9-daafdc01fd26`) was configured to operate on the
repository `swiggy-private/zone-rca-degrowing` (branch `master`). That
repository does not exist / is not accessible, so the pre_build `git fetch`
failed silently (`|| true`) and the workspace was left empty apart from `.git`.
The existing RCA dashboard file it references could not be found anywhere, so
the requested *update* could not be applied on top of an existing file.

## Where the source was searched

| Location | Result |
| --- | --- |
| Origin fetch `https://github.com/swiggy-private/zone-rca-degrowing.git` | `remote: Repository not found` (GitHub 404) |
| GitHub REST API `repos/swiggy-private/zone-rca-degrowing` (with `GIT_TOKEN`, `gh` CLI) | 404 Not Found |
| Full org listing: all 3,810 repositories in `swiggy-private` (org is visible to the CI token) | no `zone-rca-degrowing`; no close variant (`zone-rca`, `rca-zone`, `zone-rca-dashboard`, `rca-dashboard`, `zone-degrowing`) |
| GitHub code search, org-scoped: `zone vs city`, `zone cards`, `zone OPD`, `L4W`, `zone-rca-degrowing` | 0 results for this dashboard |
| All repositories under the CI account `shuttle-ci-user1` (138 repos) | no zone RCA dashboard repo |
| Swiggy KB repo-knowledge base (discovery over routing candidates) | no repo/unit/flow owning a zone RCA dashboard with these views |
| Glean (docs, Slack, Jira, artefact gallery) | zone RCA artefacts exist (e.g. `zone-rca-7cities-opszone` by kavya.k3) but no source repo; the published artefact is a snapshot behind SSO login |
| Phoenix Elasticsearch (`phoenix-metrics-*` all months; `phoenix-slack-actions` all 576 actions, 154 distinct repos; `phoenix-slack-sessions`) | no prior request ever targeted `zone-rca-degrowing` or a comparable zone RCA dashboard |
| S3 `ci-artifactstores` — request prefix `phoenix-resource/code-modernization/97422ae3-.../` | only `prompt.txt` and `buildspec-claude-code-gpt.yml`; no source bundle |
| Build workspace and full container filesystem | empty apart from `.git` |

## What this PR contains instead

This repository (`shuttle-ci-user1/zone-rca-degrowing`) was created because the
CI account cannot create repositories inside `swiggy-private` (same handling as
the earlier `local-dashboard` and `swiggy-west-rca-dashboard` requests). It
carries a **fresh, self-contained implementation** of the RCA dashboard with
every view named in the task:

- `index.html` — zone RCA dashboard for degrowing zones with:
  - **WOW trend view** — last 5 weeks per zone with per-cell sparkbars, WoW
    delta and status pills;
  - **MOM trend view** (new) — identical, comparable layout over the last 5
    months, with MoM delta and status pills;
  - **L4W / WTD / MTD views** — zone OPD and **zone-vs-city comparison** stay
    visible in all three: zone OPD, delta vs base, city OPD, zone share of
    city, zone vs even-share expectation, and top RCA driver in one table,
    plus zone cards below;
  - **summary comparison table with expandable rows** — every zone row expands
    in place (click a row) to show the zone's WOW trend, MOM trend, zone-vs-city
    table, and top RCA buckets, so a zone no longer has to be hunted by
    scrolling through zone cards;
  - city / status / text-search filters over every view;
  - embedded sample data (8 zones across 4 cities) — swap the `DATA` block for
    the weekly pipeline output, no code change needed.

## How to complete this task on the real source

1. Restore or share `swiggy-private/zone-rca-degrowing` with the
   `shuttle-ci-user1` CI account (it has org access, but the repo 404s), or
   push the source to a repo the account can read.
2. Re-run the request for `zone-rca-degrowing`; the pipeline will then fetch
   the existing dashboard and these three changes can be re-applied on top of
   the original file:
   - add an MOM trend view comparable to the existing WOW trend;
   - keep zone OPD and zone-vs-city comparison visible in the L4W/WTD/MTD
     views;
   - add expandable rows in the summary comparison table.
