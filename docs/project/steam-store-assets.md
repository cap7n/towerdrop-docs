# Steam Store & Library Art Assets

Every graphical asset Steam needs for **Tower Drop** (App ID **4987130**), with exact
sizes and what each is for. Doubles as a build checklist. Specs pulled from the
Steamworks docs (store assets + library assets), 2026-07.

See also: [Asset Pipeline](../tech/asset-pipeline.md) and the RBL ship-prep note in the
[Backlog](backlog.md).

!!! danger "RBL: never show unlicensed assets in public art"
    Capsules, screenshots and the trailer are **public display** of whatever is in them.
    They must **not** show the Roll-Call placeholder assets (PureNature rocks/grass, JMO
    CartoonFX textures, the skyboxes). Either capture around them or finish the **RBL swap
    first** (rock item models, rock atlas, grass). Capsules should be **custom key art**,
    not screenshots, anyway. Inventory: `Imported/RollCall/RBL_ASSETS.md`.

---

## 1. Store capsules (shopper-facing)

Uploaded under **Store Admin -> Graphical Assets**. PNG or JPG. Carry only the game
title/logo, no review quotes or extra text.

| Asset | Size (px) | Where it appears |
|---|---|---|
| **Header capsule** | **920 x 430** | Top of the store page, wishlists, Big Picture |
| **Small capsule** | **462 x 174** | Search results, top sellers, new-release lists (logo must read tiny) |
| **Main capsule** | **1232 x 706** | Store homepage carousel / featured spots |
| **Vertical capsule** | **748 x 896** | Seasonal sales, daily deals, sale pages |
| Page background | 1438 x 810 | Store page backdrop. Optional (auto-generated from a screenshot if omitted) |

## 2. Screenshots & trailer

| Asset | Spec |
|---|---|
| **Screenshots** | Minimum **5** (aim 6-8). **1920 x 1080** (16:9). Must be **real gameplay**. Mark **>= 4 as "suitable for all ages"** so they can appear on the store homepage. |
| Trailer / video | Optional but **strongly recommended**. MP4, 1080p+. A real gameplay trailer beats a stills slideshow for wishlists. |

## 3. Library assets (owner-facing)

Shown to people who **own or wishlist** the game, inside their Steam Library. Set via the
app's Steamworks settings (**Edit Steamworks Settings -> Graphical Assets**). All PNG.

| Asset | Size (px) | Purpose / notes |
|---|---|---|
| **Library capsule (vertical)** | **600 x 900** | Primary art in the Library grid / collections |
| **Library header** | **920 x 430** | Recent Games and similar. Falls back to the store Header capsule if omitted |
| **Library hero** | **3840 x 1240** | Banner at the top of the library detail page. **Safe zone 860 x 380 centred; NO text** (the logo overlays separately) |
| **Library logo** | up to **1280 x 720** | Transparent-background PNG logotype, overlaid on the hero. You position it (corner/centre) in the preview tool |

## 4. Icons

Set in the app's Steamworks settings, not the store page. Confirm exact requirements
there; commonly:

| Asset | Size | Notes |
|---|---|---|
| Community icon | 184 x 184 PNG | Friends list, community hub |
| Client icon | 32 x 32 `.ico` (+16 x 16) | Desktop shortcut / taskbar |

---

## Where each is uploaded

- **Store capsules, screenshots, trailer** -> Store Admin -> **Graphical Assets** / **Trailers** tabs.
- **Library assets + icons** -> the app's **Steamworks Settings** (Edit Steamworks Settings -> Graphical Assets / General).

## Minimum to launch the "Coming Soon" page

To pass Valve review and go public for wishlists you need at least: the **store capsules**
(header + small + main + vertical) and **>= 5 screenshots**, plus the short + full
description. **Library assets and the trailer can follow.** So the art critical path is:
custom capsules + a clean set of screenshots (post-RBL-swap).

## Build checklist

Store:

- [ ] Header capsule (920 x 430)
- [ ] Small capsule (462 x 174)
- [ ] Main capsule (1232 x 706)
- [ ] Vertical capsule (748 x 896)
- [ ] Page background (1438 x 810) *(optional)*
- [ ] 5+ gameplay screenshots (1920 x 1080), 4+ tagged all-ages
- [ ] Trailer (MP4 1080p+) *(recommended)*

Library:

- [ ] Library capsule (600 x 900)
- [ ] Library header (920 x 430)
- [ ] Library hero (3840 x 1240, safe zone 860 x 380)
- [ ] Library logo (<= 1280 x 720, transparent)

Icons:

- [ ] Community icon (184 x 184)
- [ ] Client icon (.ico 32 x 32)

Gate before any of the public ones:

- [ ] **RBL swap done** (rock models, rock atlas, grass) so screenshots/trailer are clean
