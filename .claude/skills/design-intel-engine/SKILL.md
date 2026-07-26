---
name: design-intel-engine
description: Reference plan and targets for building an automated design-intelligence pipeline that studies reference designers/studios (X profiles, portfolios, sites) and distills their visual systems (palette, type, grid, motion, logo construction) into reusable tokens, a dossier library, and a self-eval loop — so future design work can match or exceed their level. Use when the user asks to continue, scale, or implement the "AI DESIGNERS" / design-intel-engine project, or to add new target designers to study.
---

# Design Intel Engine

Full plan: see `docs/design-intel-plan.md` in this repo (also mirrored at
`/root/.claude/plans/necesito-que-me-crees-encapsulated-fox.md` when in the
original session).

## Goal

Build capability, not a copy machine. Weekly automated ingestion of reference
designers' public work (X posts/media, websites, portfolios) feeds an
extraction pipeline (palette, typography, layout, motion, logo construction)
whose output is a `library/SYSTEM.md` + `tokens.json` + a compiled
`skill/design-master/SKILL.md` I load when designing. A self-eval loop
produces test pieces against briefs, scores them against the references, and
feeds the gap back into the skill — that's the real, measurable improvement
mechanism (no weight fine-tuning happens; the "training" is the artifact
loop itself).

## Hard rule on sourced assets

Anything downloaded from a target (images, logos, site captures) is
reference material only, stored under `store/raw/` with a source manifest
(url, author, date, hash), never committed, never republished. Deliverables
are generated only from the distilled `SYSTEM.md`/`tokens.json` — principles
and parameters, not the original files. The eval rubric explicitly penalizes
outputs that resemble a reference too closely (originality is a scored
metric, not an afterthought).

## Known constraint

This session's remote sandbox proxy blocks outbound connections to the
target domains (403 on CONNECT — environment network policy, not a site
issue). Ingestion (`ingest/x_profile.ts`, `ingest/website.ts`) must run
somewhere with open network access: the user's local machine via Claude
Code CLI, or a freshly created remote environment with an unrestricted
network policy. Code/scaffolding can still be written and pushed from here.

## Targets collected so far

**Tier A (X profile + own site/portfolio — full pipeline):**
- @thetimgabe
- @designbyzipzap → strategy.zipzap.design (site sells *strategy*, study the offer)
- @yahyavision → yahyavision.com, cal.com/yahyavision/30min (full funnel, **pilot target**)
- @ktnr23 → dribbble.com/ktnr
- @gola99 → gola.supply
- @driceroland → nor.ma (verify authorship)
- @kyleanthony
- @davidmokos_ → steercode.com (product/dev-tool reference, not a studio)

Seed posts (anchored "this is the bar" examples):
- x.com/yahyavision/status/2080987176305926611
- x.com/ktnr23/status/2068241431504798152

**Tier B (site/studio, no linked X — web+visual pipeline only):**
- matesha.studio
- studio.morflax.com (motion/3D — high value for extract/motion)
- jckhlry.com/contact (also useful for studying how they close a sale)
- nor.ma

**Tier C (pattern/business-model corpus, not direct emulation targets):**
- the-brandidentity.com — editorial branding publication, richest single
  source for an identity-design corpus (many cases with written rationale)
- dribbble.com/KateHets
- framer.com/community/marketplace/templates/kanso — how a design product is
  packaged and sold
- amirmushich.gumroad.com — monetization model for digital design products
- moda.app/s/Opm4dJfiGiW-JRJ6AeQk3A — shared link, classify on open

## Next concrete step

F0 bootstrap of a new dedicated repo `design-intel-engine` (TypeScript +
Playwright scaffolding, `targets.yaml` populated from the table above,
`.gitignore` excluding `store/raw/`, `.env`, `storageState.json`), then F1
pilot ingestion on @yahyavision — but only once ingestion has a place to run
with open network access (see constraint above) and the user provides a
Playwright `storageState.json` for an authenticated X session.
