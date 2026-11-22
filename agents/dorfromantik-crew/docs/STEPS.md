# The Four Steps

One step per session. Each step is its own branch and its own stacked pull request. Docs named
below are looked up in the target repo's `docs/` by topic — file names there may differ; the
project `CLAUDE.md` lists the current ones. Read only the docs of the step you are in.

| # | Step | Branch slug |
| --- | --- | --- |
| 1 | Progress tree, base game | `progress-tree-base` |
| 2 | Base game completed | `base-game-complete` |
| 3 | Variant Sakura | `variant-sakura` |
| 4 | Nextcloud scaffold and PWA | `nextcloud-pwa` |

## Step 1 — Progress tree, base game

**Docs:** progress tree and unlocks (base game), the `.drawio` diagram of the score block, rules
summary, the base-game specification summary.
**Scope:** the graph as JSON; path logic (choice at forks, automatic crossing, crossed fields cost
nothing, path continues beyond the cloud, no dead end, optional leftover fields); automatic
entries in the campaign sheet ("Eure Erfolge", campaign area); red hearts. Align the roadmap in
the project `CLAUDE.md` to this order.
**Done when:** every path rule has a test; a campaign recomputes tree and entries purely from raw
data; `npm test` is green; the prototype UI shows the tree.

## Step 2 — Base game completed

**Docs:** app requirements (campaign management), rules summary.
**Scope:** start screen with new, running and finished campaigns; end a campaign; finished
campaigns read-only (view, delete); correction of closed score sheets with a warning and
recomputation.
**Done when:** the campaign life cycle is covered by tests in the core; correcting a closed sheet
recomputes tree, unlocks and entries; the warning is shown before the correction.

## Step 3 — Variant Sakura

**Docs:** the Sakura docs (score sheet, progress tree, unlock rules), `graph-sakura.json`.
**Scope:** Sakura score sheet, graph, achievement list with its automatisms, cherry-blossom
dialog with spending budget — all in the pure core, driven by the variant chosen at campaign
creation. Extend `web/` only as far as needed to try it out.
**Done when:** the variant selects sheet, graph, highscore and achievement list; every Sakura rule
has a test; base-game behaviour is unchanged.

## Step 4 — Nextcloud scaffold and PWA

**Docs:** app requirements (deployment, persistence, PWA), project `CLAUDE.md` (Nextcloud
sections).
**Scope:** a `docker compose` file with the official Nextcloud image into which the app is
mounted, PHP backend with storage per campaign (raw data only), Vue frontend running the
unchanged core, PWA manifest and service worker, install instructions for the family (home
network, VPN from outside; no app store, no accounts of its own).
**Done when:** the app installs into the container instance; a campaign survives a reload and a
restart; the core is unchanged from what steps 1–3 produced; the PWA is installable (manifest and
service worker valid); integration tests pass against the container. What cannot be verified
without a human on a phone is listed in the pull request.
