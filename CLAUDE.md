# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the website for **נוף בחצר** (Nof Bahatzer) — a wine bar and shop in Hod HaSharon, Israel. It is a **single-file static website** with no build tools, no package manager, and no framework.

## Architecture

The entire site lives in `index.html`. All CSS is in `<style>` blocks and all JavaScript is in `<script>` blocks inline within that file. There is no external JS or CSS besides Google Fonts.

### Layout sections (in order)
1. **Hiring popup** — fixed overlay shown on load, dismissed by button
2. **Top bar** — sticky header with logo and anchor navigation
3. **Hero** — fullscreen `<video>` with overlay and CTA buttons
4. **About** (`#about`) — two-column text + image grid
5. **Wine & Kitchen** — two-column with image flipped left via `direction: ltr` trick
6. **Gallery** (`#gallery`) — JavaScript carousel with touch/swipe and dot indicators
7. **Info** (`#info`) — four-column grid (hours, reservations, parking, happy hour)
8. **Final CTA** — green band with reservation links
9. **Footer** — logo, address, contact links (phone, WhatsApp, Instagram, Facebook, Waze)

### Design system (CSS custom properties)
```
--beige / --beige-light / --beige-dark   background tones
--green / --green-dark                   primary brand colour
--text / --text-soft                     body text
--line                                   border/divider colour
--white
```
Headings use **Frank Ruhl Libre** (serif). Body text uses **Heebo** (sans-serif). Both loaded from Google Fonts.

### Responsive breakpoints
- `≤ 900px` — nav hidden, single-column layouts, smaller hero
- `≤ 520px` — single-column gallery, stacked CTA buttons

### Carousel (vanilla JS)
Located in the gallery section. Shows 3 slides on desktop, 1 on mobile (`≤ 700px`). Translates the track with `translateX` in px based on live `getBoundingClientRect().width`. Dot navigation and touch swipe are supported.

## Development

No build step required — open `index.html` directly in a browser or serve with any static file server:

```bash
python3 -m http.server 8080
# or
npx serve .
```

There are no tests, no linter, and no CI configuration.

## Language & Content

The site is fully in **Hebrew**, written right-to-left (`<html lang="he" dir="rtl">`). The `wine-kitchen` section overrides this with `direction: ltr` on the two-column wrapper to flip image placement, then re-applies `direction: rtl` on the text block child.

## External Links

- Reservations: `https://ontopo.co.il/nofbahatzer`
- Menu: `https://hopa.tech/nof-bahatzer`
- WhatsApp contact: `https://api.whatsapp.com/send?phone=9720542364455`
- Social: Instagram `nof_bahatzer`, Facebook `NofBahatzer`
