---
name: surface-web-craft-kit
description: >-
  Use when packaging or shipping the SURFACE sellable web-craft skill pack
  (taste/motion/liquid materials), building its cross-linked portal showcase
  sites, or producing the Xiaohongshu/social sales carousel for that pack.
---
# SURFACE · 高级网页表面技能包

## Goal
Deliver a **sellable** agent skill pack for premium web surfaces (taste + type/motion + liquid materials) plus multiple exquisite portal sites that cross-link and sell the pack, then a sales post pack (screenshots + copy). Do not stop at a skill list.

## Canonical paths
- Pack root: `<LOCAL_DEMO_PATH>`
- Portals: `<LOCAL_DEMO_PATH>`
- Sell post: `<LOCAL_DEMO_PATH>`
- Index: `PACK/SKILL_INDEX.md` · Sales: `PACK/COPY_SALES.md` · Manifest: `PACK/MANIFEST.json`

## Skill contract (primary)
1. Read `PACK/SKILL_INDEX.md` and the specific skill under `PACK/skills/<name>/SKILL.md` before coding a portal.
2. Primary craft for portals: `design-taste-frontend` + `high-end-visual-design` + (`liquid-glass` | `liquid-metal` | `motion-pages` pura) — pick one hero craft per portal so sites are not clones.
3. References: Claude Directory craft, motion-pages examples, existing yc-lab E/H/I (liked) — avoid repeating failed J/K weaknesses (weak mobile, thin content).

## Hard taste
- Bright/bone/paper OK; single accent; **no acid yellow**; **no** dark Agency + huge type + Lenis pin universal skeleton.
- Mobile-first: `@media` 720/480, safe-area, ≥44px targets, no horizontal overflow.
- **Portal copy (t50u):** Ban on-page jargon — no `SAMPLE≠授发`, `完成≠授发`, `授权不等于发`, `完成可预览≠…`. Those stay in team HANDOFF only. Page text = real PACK skill names / descriptions / hard rules / step titles / short code. Author line may be a minimal `yc`. Four portals must use **distinct hero crafts** (index / refraction lab / metal+type / bento fragments) — never the same hero+cards+CTA skeleton.
- No unprompted disclaimer microcopy on visuals.

## Portal set (each can be the homepage)
Build ≥3 (prefer 4) single-file-prefer HTML portals, host on deed.page:
1. **atlas** `surface-atlas` — bone/paper **magazine skill index** (filterable, expandable SKILL.md excerpts); not a marketing hero
2. **lens** `surface-lens` — **interactive liquid-glass lab** (draggable lens over skill card wall)
3. **ingot** `surface-ingot` — **WebGL liquid-metal hero surface** + liquid/ripple type (compact casket; not Agency long-scroll)
4. **board** `surface-board` — bright **Bento of skill fragments** (rules/chips/code/palette/portal thumbs); zero slogan wall
Every portal: mutual links + mobile OK + `export/preview.png` + `preview-mobile.png`. Sales jargon only in sell-post HANDOFF.

## Publish
Use deed.page token `<REDACTED_TOKEN_PATH>` and existing `publish-large.mjs`. Curl verify 200. Write `PORTALS.md` with live URLs.

## Sales post
After portals live: capture desktop+mobile screenshots; compose 小红书 3:4 carousel (1080×1440) in `sell-post/` + captions from `COPY_SALES.md` (run viral-copy-zh + de-ai-zh). **Do not auto-publish** to social; hand off assets (SAMPLE≠授发).

## Report
Live portal URLs, pack path, sell-post folder, which skills each portal demonstrates.
