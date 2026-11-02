---
title: Branding
nav_order: 5
---

# Branding & customization

The wallet's name, accent colors, font, and language are all
configurable without a rebuild — a deployment sets them once and the
popup applies them at every startup. This covers the *in-app* UI only;
see "What can't be changed this way" below for the one MV3 constraint
that doesn't move.

## Two ways to set it

**1. Import a config from Settings (no rebuild)** — open the popup →
Settings → Branding → **Import branding**, paste a JSON config (schema
below), Apply. It's validated on the spot and takes effect immediately.
**Reset to default branding** reverts to whatever's shipped in the
build.

This writes to `chrome.storage.local`, scoped to this one installed
copy of the extension — it doesn't touch the packaged files, so it
doesn't survive an uninstall/reinstall, and it doesn't propagate to
anyone else's install.

**2. Change the shipped default (rebuild required)** — edit
`DEFAULT_BRAND` in `src/shared/brand.ts`, then
`pnpm build` / `pnpm package`. Use this when you want every fresh
install of a build to start already branded, not just one that someone
manually imported a config into.

Both paths accept the same shape.

## Config shape

```json
{
  "name": "Acme Wallet",
  "font": "poppins",
  "locale": "en",
  "colors": {
    "light": { "--accent": "#0078d4", "--accent-gradient-button": "#0078d4" },
    "dark": { "--accent": "#4cc2ff", "--accent-gradient-button": "#4cc2ff" }
  }
}
```

All fields are optional — set only what you want to change; anything
omitted keeps its current value.

| Field | Type | Notes |
|---|---|---|
| `name` | string, ≤60 chars | Shown in the header, the popup's browser-tab title, and interpolated into a few copy strings ("Paste it here — Acme Wallet handles the rest"). |
| `font` | string | Must be a key in `FONT_REGISTRY` (`src/shared/brand.ts`) — currently `"poppins"` (the shipped default) or `"raleway"`. See "Adding a font" below. |
| `locale` | string | Must be a code in `AVAILABLE_LOCALES` — currently `"en"` or `"de"`. Sets the *default*; a user can still switch languages themselves in Settings regardless of this value. |
| `colors.light` / `colors.dark` | object | CSS custom-property name → value. Only the keys in the table below are accepted — anything else is rejected. Values must match `[a-zA-Z0-9#().,%\-\s]+` (hex colors, `rgb()`/`rgba()`, keywords, percentages, `repeating-linear-gradient(...)` all work; `;`, `{`, `}`, `<`, `>` don't — this is what stops a pasted config from breaking out of its own CSS declaration). |

## Color tokens

Two tiers, and the distinction matters — a lot of the visible "does
this actually look branded" impact lives in the second one, not the
first:

| Token | Controls |
|---|---|
| `--accent`, `--accent-hover` | Links, active tab-bar icon, small text-scale accents |
| `--accent-gradient` | Avatars, checkboxes, active filter chips, icon circles — the actual fill behind white icon content |
| `--accent-gradient-button` | Primary button fill — the single highest-visibility brand surface in the UI |
| `--accent-gradient-button-fg` | Primary button's label color. Defaults to white if unset (today's behavior) — set this when the button fill is light enough that white fails (e.g. a brand whose primary color is a bright yellow needs a dark label instead) |
| `--orb-gradient-alt` | Second-issuer avatar color, for visual variety between credential groups |

Despite the names, `--accent-gradient`/`--accent-gradient-button` accept
a flat color just as well as a `linear-gradient(...)` — set a plain hex
value if your brand (like sideos's) rejects gradients outright.

`--accent-gradient`'s white icon content is still hardcoded at every use
site (Avatar.tsx and friends), so pick a value dark or saturated enough
to hold white at a reasonable contrast there; a light accent hue will
wash out. `--accent-gradient-button` no longer has that constraint — its
label color is `--accent-gradient-button-fg` (see above), so a bright,
light primary color (a bright yellow, say) can pair a dark label with a
light fill instead of being forced into a darker fill just to stay
legible.

| Token | Controls |
|---|---|
| `--accent-tint-bg`, `--accent-tint-fg` | Tag pills, credential-count badge, highlighted rows |
| `--ink`, `--ink-2` … `--ink-5` | Text, darkest to lightest (headings → placeholder-level) |
| `--surface`, `--surface-2`, `--surface-3` | Card and pill/input background |
| `--surface-header` | CredentialDetail screen's top pane specifically — its own token, separate from `--surface-2`/`-3`, so it can be recolored without affecting other cards |
| `--aurora-page`, `--aurora-page-warn`, `--aurora-page-ok` | Page background — the default screen, the RequesterWarning screen, and the PresentationSent screen each use their own variant, so move all three together or one screen keeps the old wash |
| `--hairline`, `--hairline-2`, `--border` | Dividers and input borders |
| `--toggle-off`, `--check-border` | Off-state switch track, unchecked checkbox border |
| `--success-bg`, `--success-fg`, `--success-fg-strong`, `--success-fg-mid` | Trusted-issuer/verifier banners |
| `--danger-bg`, `--danger-fg`, `--danger-fg-strong`, `--danger-fg-mid` | Warnings, blocked requesters, delete/reset actions |
| `--muted-avatar` | Expired-credential avatar color |
| `--chevron` | List-row disclosure arrows |
| `--overlay`, `--overlay-sheet` | Modal/sheet backdrop tint |
| `--placeholder-stripe`, `--placeholder-border` | Empty-state illustration |
| `--grab-handle` | Bottom-sheet drag handle |
| `--shadow-sheet` | Bottom-sheet drop shadow — set to `"none"` if your brand rejects drop shadows |

Set only `light`, only `dark`, or both — an unset mode just keeps
theme.css's own default for that token, which is how the wallet ships
today. Deliberately **not** in this list: `--radius-*`,
`--chrome-blur`/`--glass-blur`/`--scrim-blur` — those are genuinely
shape/rendering-effect properties, not color, and stay the Aurora Glass
design system's own choice regardless of brand config. `--aurora-page`
looked like it belonged in that same "structural" bucket at first
(its name, and its gradient default) but it's really just page
background color wearing a gradient by default — same reasoning as
`--accent-gradient-button` above, and it's now overridable the same
way. A config can recolor the wallet, including its background; it
can't turn it into a flat, square-cornered design
system — that would mean building an actual alternate theme, not
importing a config.

## Adding a font

Only fonts actually bundled via `@fontsource` can be selected — an MV3
popup's CSP makes loading fonts from a remote URL impractical, so this
is "pick from what's shipped," not "point at any font." To add one:

1. `pnpm add @fontsource/<font-name>` and import its weights in
   `src/popup/main.tsx` (see the existing Poppins
   / Raleway / IBM Plex Mono imports — latin subset only, to avoid
   pulling in unicode ranges this UI never renders).
2. Add an entry to `FONT_REGISTRY` in
   `src/shared/brand.ts` mapping a key to a
   CSS `font-family` stack.
3. Rebuild. The new key is now a valid `font` value in any config.

## Adding a language

1. Copy `src/popup/locales/en.json` to a
   new `<code>.json` in the same folder and translate every value
   (keys must match exactly).
2. Register it in `CATALOGS` in
   `src/popup/components/LocaleProvider.tsx`
   and add it to `AVAILABLE_LOCALES` in
   `src/shared/brand.ts`.
3. Rebuild. It now shows up in Settings → Language, and as a valid
   `locale` value in any config.

A key missing from a non-English catalog falls back to English rather
than showing blank or the raw key, so a partial translation is safe to
ship.

## What can't be changed this way

The extension's own name and icon — the ones shown in
`chrome://extensions`, the toolbar, and (if it were ever published) a
Web Store listing — are set in
`manifest.config.ts` and locked in at package
time; that's a Chrome/MV3 constraint, not a gap in this system. Change
`DEFAULT_BRAND.name` there and rebuild if you want that to match too.
The bundled toolbar icon (`public/icons/`) isn't swappable at runtime
either, for the same reason.

## Security note

A pasted config never reaches the DOM or storage unvalidated —
`parseBrandConfigInput` in
`src/popup/lib/brand.ts` allowlists every
field (known color tokens only, safe-value pattern only, known
font/locale codes only) before anything is applied. There's no
remote-fetch path — configs are only ever supplied locally (pasted in
Settings, or baked in at build time). Wiring this up to fetch a
deployment's config from a remote URL is a real feature someone could
want, but it's a separate decision with its own trust-model questions
(who's allowed to serve a config, how it's authenticated) — not
something this system does today.
