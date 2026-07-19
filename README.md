# Playable Ads

**Live portal (share with the team):** https://gilmisgav.github.io/vantage21-playables/
— gallery at `/`, device-resolution preview at `/viewer.html`. Every push to
`main` on [GilMisgav/vantage21-playables](https://github.com/GilMisgav/vantage21-playables)
redeploys the site automatically (GitHub Pages).

Single-file HTML5 playable ads for Meta (Facebook) and other networks. Each
playable is fully self-contained — no external requests (the Vantage 21
creative embeds the real app card art + Cormorant Garamond as data URIs).

## Folder layout

```
playables/
├── index.html          ← Gallery: browse & preview every variation (start here)
├── studio.html         ← Editor: Visual + Code + AI + per-network Export
├── frame.html          ← Phone-frame viewer (used by the gallery's "Frame" button)
├── README.md           ← This file
├── assets/             ← (optional) shared snippets / notes
├── variations/         ← One .html file per playable ad
│   └── vantage21-bet-hit-21.html
└── ugc/                ← UGC creatives: animatics (.html) + scripts (.md)
    ├── split-screen-win-reaction.html   (animated video / animatic)
    └── split-screen-win-reaction.md     (full shoot script)
```

The Gallery has a **🎬 UGC creatives** section below the playables. Animatics are
self-contained animated HTML "videos" (the script played as motion: split-screen,
ticking balance, captions, CTA) — usable as motion creatives, screen-recorded to
MP4, or as the animatic to brief a real creator shoot. Register a UGC creative by
adding an entry to the `UGC` array in `index.html` (fields: `title`, `file` (html),
`script` (md), `desc`, `tags`, `length`, `status`).

> Note: true live-action UGC needs a real creator on camera — the animatic is the
> motion mockup/brief, not filmed footage.

## The Studio (editing each playable)

From any gallery tile, click **✦ Edit in Studio**. It opens `studio.html` with a
live phone preview and four tabs:

- **🎨 Visual (Canva-style)** — form fields for branding, theme colors, economy
  (balance / bet chips / payout), the card scenario, all on-screen copy, and the
  network. Edits update the preview instantly.
- **✏️ Edit on canvas** — click *Edit on canvas* under the preview, then work
  directly on the phone:
  - **Click** an item to select it → a toolbar appears with **✏️ edit text**,
    **✨ AI change**, **🗑️ delete**.
  - **Double-click** text to edit it inline; **A− / A+** on the toolbar change its
    size; **drag** any item to reposition it.
  - **Every asset is a separate object** (Keynote-style) — chips, cards, buttons,
    scores, labels, balance, text: click any one to select, **drag to move**,
    **A−/A+** to resize text, the **color swatch** to recolor text, ✨ to AI-change,
    ✕ to delete. Position offsets use the CSS `translate` property so they never
    fight the game's animations.
  - **Box-select a group** — drag on empty felt to draw a marquee; every visible item
    inside is selected, then drag any of them to **move the whole group together**.
  - **➕ Text** adds a new text layer; **🖼️ Image** uploads a picture (embedded as a
    data-URI so the ad stays self-contained — keep images small); **✨ AI element**
    generates a new styled asset (badge/sticker/text) from a prompt and drops it on
    the canvas (runs live with an API key, or copy-brief without one).
  - **✨ AI change** on a selected item: type what to change; runs live if you set an
    API key, otherwise copies a focused brief to paste into Claude. *(Claude rewrites
    text/markup/styles — it does not generate images; upload those.)*
  - All edits persist into `CONFIG` (`layout` positions, `hidden` deleted ids,
    `customItems` added text/images) and travel into every export. **Reset edits**
    clears them.
- **⌨️ Code** — the full single-file source. Edit by hand, *Apply & Preview*, or
  *Rebuild from Visual*. Download the current file.
- **✨ AI** — describe a change or ask for a review. Two ways:
  - **Copy brief → paste in Claude** (no setup): copies your file + instruction so
    you can paste it into Claude Code and get the updated file back.
  - **Run live** with your own Anthropic API key (kept in your browser's
    `localStorage`, never written into exported ads). Edits replace the Code tab;
    reviews print inline.
- **⬇️ Export** — download a build per network with the correct CTA hook, plus a
  pre-flight check that scans for external assets / network calls and reports size.

### Saving & version history

- Edits **auto-save in your browser, per creative** (keyed by file path), so they
  survive reloads — the topbar shows **Saved ✓**. The source file in `variations/`
  is never overwritten unless you export/download and replace it yourself.
- The **🕘 History** tab lists every checkpoint (auto-snapshots on each action plus
  any **Save named version**). **Restore** rolls back to any point (and the restore
  itself is undoable). **Discard saved & reset to original** clears the saved copy
  and history for that creative.
- Saving uses `localStorage`, so it's per-browser/per-machine (not synced). Large
  embedded images can hit the storage limit — the indicator warns if a save is too
  big, and old history entries are pruned automatically.

### How configuration works

Each playable has a `CONFIG` block near the top of its `<script>`, between
`//CONFIG_START` and `//CONFIG_END`. The Studio reads and rewrites just that block,
so the file stays a single self-contained ad. You can also edit `CONFIG` by hand.

### Networks & CTA hooks

| Network    | Export sets `CONFIG.network` | CTA call on click |
|------------|------------------------------|-------------------|
| Facebook   | `facebook`                   | `FbPlayableAd.onCTAClick()` |
| AppLovin   | `applovin`                   | MRAID `mraid.open(url)` (falls back to `window.open`) |
| Apple      | `apple`                      | `window.open(url)` — *Apple Search Ads has no playable format; use for App Store landing / Apple display* |
| Generic    | `generic`                    | `window.open(url)` — base for Unity / ironSource / Google |

## Make a new creative with AI (home page)

On the Gallery, click **✨ New creative with AI**. Describe the product/angle, pick a
**format**, and generate:

- **🎮 Playable** — a full single-file interactive HTML5 mini-game with the correct
  network CTA hook.
- **🖼️ Regular** — a static banner / interstitial creative (HTML, no interaction).
- **🎬 UGC** — a user-generated-content ad concept: hook, script, shot list, captions, CTA.

Pick the **network** (Playable/Regular) and **model**. With an Anthropic API key it
generates inline (preview + Download); with no key, **Copy brief** drops a ready
prompt to paste into Claude Code. Generated Playables can be saved into
`variations/` (and registered) to join the gallery; UGC outputs download as `.md`.

## How to view

Open `index.html` in a browser (or via the local server). It shows a live,
playable thumbnail of every variation. From a tile you can:

- **Open / Play full-screen** — the raw playable, exactly as Meta runs it.
- **Phone frame** — the playable inside a phone mockup with a *Replay* button (great for QA).

## How to add a new variation

1. Duplicate an existing file in `variations/` (e.g. copy `vantage21-bet-hit-21.html`)
   and rename it, e.g. `vantage21-double-down.html`.
2. Edit the game logic / scenario in that file. Keep everything in the one file.
3. Register it in the gallery: open `index.html`, find the `VARIATIONS` array
   near the top of the `<script>`, and add one object:

   ```js
   {
     title:   "Vantage 21 — Double Down",
     file:    "variations/vantage21-double-down.html",
     game:    "Blackjack",
     desc:    "Short pitch of what happens in this variation.",
     tags:    ["blackjack", "double-down"],
     status:  "draft",          // "live" when it's ready to ship
     updated: "2026-06-22",
   },
   ```

That's it — the tile (with live preview) appears automatically.

## Naming convention

`variations/<game>-<hook>.html` — lowercase, hyphenated.
Examples: `vantage21-bet-hit-21.html`, `vantage21-double-down.html`, `vantage21-split-aces.html`.

## Meta playable requirements (checklist)

- ✅ **Single file** — all HTML/CSS/JS inline; no external requests.
- ✅ **No network calls** — Meta strips them; everything must be local.
- ✅ **Size** — keep under ~2 MB (these are ~25 KB each).
- ✅ **CTA** — calls `FbPlayableAd.onCTAClick()` (injected by Meta at runtime;
  falls back to a console log when previewed standalone).
- ✅ **Orientation** — built for portrait phone (max 480×854).
- ✅ **Fast** — interactive within ~15–20s of gameplay before the end screen.

To ship: upload the single `variations/*.html` file directly in Meta Ads
Manager's playable-source field. Do **not** upload `index.html`/`frame.html` —
those are internal tooling for browsing/QA only.

## Current variations

| File | Game | Hook | Status |
|------|------|------|--------|
| `vantage21-bet-hit-21.html` | Blackjack | Choose stake → reveal 5 → hit to 21 → dealer busts | live |
| `bjswitch-build-your-hands.html` | Blackjack Switch | Two weak hands (16 & 15) → swap 2nd cards to build 20 & 11 → bet ×2 → stand + double to 21 → double win → PLAY NOW | live |
| `vantage21-what-would-you-do.html` | Blackjack | **FB-ready.** Real app assets (card PNGs from the Xcode bundle, Cormorant Garamond + app icon from vantage21.gg, shoe top-right). "What would you do?" → HIT/DOUBLE/SPLIT/STAND → PLAY NOW. Built-in ✎ scenario editor (hands, text, buttons, per-action draws) | live |
