# Port St. Lucie Healthcare Network

A single-file web app that helps someone find real healthcare providers serving Port St. Lucie, FL, across 14 specialties:

Cardiology, Gastroenterology, Dentistry, ENT / Otolaryngology, Primary Care, Urology, Gynecology / OB-GYN, Dermatology, Orthopedics, Pediatrics, Ophthalmology, Psychiatry / Mental Health, Physical Therapy, and Urgent Care.

This project is modeled on the architecture, visual design, and sourcing discipline of a companion project, the Port St. Lucie Contractor Network.

## What it is

- A single `index.html` file — no build step, no framework. Embedded CSS (coastal teal design system, light/dark theme support) and a vanilla-JS IIFE.
- A category rail, search, rating/Google-rated filters, sort controls, a responsive card grid, a detail modal, a side-by-side compare tool (up to 3 providers), and a "Suggest a provider" community submission form.
- A ZIP-code coverage checker and summary stats (total providers, specialties covered, Google-rated vs. other-platform-rated counts).
- Favorites ("My List") and community submissions sync to a shared backend when the page is opened as a Claude Artifact (via the `db` and `user` runtime capabilities); otherwise it falls back cleanly to `localStorage` ("local demo only").

## Sourcing methodology

Every provider was found via live web search, not invented. The process for each of the 14 categories:

1. Search patterns like "best `<specialty>` doctor Port St Lucie FL", "top rated `<specialty>` Port St Lucie FL", and "`<specialty>` Port St Lucie FL Google reviews."
2. Cross-checked against doctor-rating platforms: Healthgrades, Zocdoc, Vitals, Healthline, WebMD, Psychology Today, Sharecare, Birdeye, rater8, Tebra, Cleveland Clinic's own provider directory, and Three Best Rated.
3. **A rating is labeled "Google" only when a source explicitly attributed it to Google.** Every other rating names its real platform (Healthgrades, Zocdoc, Healthline, Cleveland Clinic, Tebra, Sharecare, rater8, Birdeye, Solv Health, Medical News Today, Beaming Health, WellMed directory, etc.) — nothing is defaulted to "Google."
4. Providers are ranked within each category primarily by rating, weighted by review count, so a 5.0 with 1 review does not outrank a 4.9 with 300 reviews. Judgment calls on ranking are documented in a per-provider `note` field whenever they weren't obvious.
5. **No category is padded to a fixed count.** Category sizes range from 4 (Dermatology) to 10 (Psychiatry / Mental Health) based purely on what could actually be sourced with a real name and a real address or clearly stated service area.
6. For each provider, the following was captured when available: name, phone, website, address, ZIP, hospital affiliation (only when a source explicitly stated one — never inferred), rating `{value, count}`, rating source platform, every source URL actually used, an optional sourcing caveat, and a short factual description.

### What was deliberately **not** verified

Board certification, license status, insurance-network participation, and current new-patient status were **not independently verified** — these cannot be reliably confirmed from search snippets, and getting them wrong for healthcare is a real-harm risk. Where a directory snippet mentions something like "board-certified," it's included in the provider's description as a direct paraphrase with attribution — never presented as a verified badge or used as a filter.

### Known limitations

- Several doctor-rating platforms (Healthgrades, Zocdoc, Vitals, WebMD, rater8, Three Best Rated, Psychology Today) blocked direct page fetches during research in this environment, so some figures come from search-result snippets rather than a live page read. These should be spot-checked against the live pages before being treated as current.
- Addresses, suite numbers, and phone numbers sometimes conflict between directories. Conflicts are flagged per provider in the `note` field where known, but were not independently resolved by calling each office.
- Many providers simply have no public star rating anywhere — those are shown honestly as "Not yet rated" rather than guessed.
- This is a single snapshot in time, not a live feed. Providers may have moved, closed, changed ratings, or changed new-patient status since this was compiled.

## Medical disclaimer

**This directory is informational only and is not medical advice.**

- Listing a provider here is **not an endorsement** and does not verify board certification, license status, insurance-network participation, or whether a provider is currently accepting new patients.
- Ratings are a single snapshot compiled from public search results, not a live feed, and may be out of date or mischaracterized by platform.
- Always confirm credentials, insurance acceptance, and current availability directly with the provider and your insurer before scheduling.
- **If you are experiencing a medical emergency, call 911 or go to the nearest emergency room — do not use this directory.**
- "Suggest a provider" submissions come from the community and are explicitly unverified until independently checked.

## Provider counts (at time of writing)

- **96 providers** across all 14 specialties
- **4** labeled "Google-rated" (rating explicitly attributed to Google by a source)
- **41** rated, but on another named platform (Healthgrades, Zocdoc, Healthline, Cleveland Clinic, Tebra, Sharecare, rater8, Birdeye, Solv Health, Medical News Today, Beaming Health, etc.)
- **51** with no public rating found — shown as "Not yet rated," never guessed

## Tech

- Single HTML file: embedded `<style>` (coastal teal design tokens, dark-mode aware) and a single vanilla-JS `<script>` IIFE at the bottom.
- Fonts: Big Shoulders Display (headings), Source Sans 3 (body), IBM Plex Mono (numbers).
- Published as a Claude Artifact with the `db` and `user` runtime capabilities for shared favorites and community submissions; works standalone with `localStorage` fallback if opened without those capabilities.
