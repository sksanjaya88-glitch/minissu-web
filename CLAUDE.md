# Minissu.lk — project context for Claude

## What this is
Minissu.lk: an AI-powered, location-based P2P marketplace connecting skilled workers (painters, masons, electricians, plumbers, AC technicians, engineers, etc.) with customers in Sri Lanka. Sinhala-first, with Tamil and English. Target users include people with low digital literacy.
Motto: කියන්න. සොයන්න. සම්බන්ධ වන්න. / Just Speak. Find. Connect.
Tagline: ඕනෑ වැඩකට මනුස්සයෙක්, දැන් ෆෝන් එකෙන්

## Current state
- Single file: `index.html` (HTML + CSS + vanilla JS, no build step). Phase 1 MVP prototype.
- Also published as a claude.ai artifact, where `claude.use("db")`, `claude.use("user")` and `claude.use("sample")` provide shared storage and AI. Outside claude.ai (`window.claude` undefined) the page falls back to: demo profiles + localStorage, rule-based profile builder, keyword search only. Keep these fallbacks working.

## Code map (index.html)
- `T` — UI strings for `si`, `ta`, `en`. Every user-facing string goes here in all three languages.
- `CATS` — service categories with multilingual + Singlish keywords (`k`). Add keywords here to improve search.
- `TOWNS_RAW` — towns: id, Sinhala, English, Tamil, district, approx lat/lng, Singlish spellings. Coordinates are approximate.
- `DEMO` — fictional sample workers, always labelled "Sample profile", never contactable.
- `detectCats`, `detectTowns`, `kwHit`, `lev` — matching (non-Latin keywords = substring; Latin = token match + Levenshtein ≤1 for words ≥5 chars).
- `rankWorkers` — score: skill 40, distance 25, availability 15, verification ≤10, experience ≤10.
- `buildProfile` / `localProfile` / `aiSearch` — AI calls with rule-based fallback.
- Views: `vHome`, `vRegister` (6-step wizard), `vFind`, `vMe`, `profileBody`. Events via `data-act` delegation.

## Rules
- AI must never invent qualifications, years of experience or certifications. Years from AI are dropped unless the number appears in the worker's own words.
- Never show a worker's exact address — town / service area only.
- Verification badges must state only what was actually checked (phone OTP is not implemented yet → "not verified"; selfie ≠ ID check).
- Large tap targets, minimal typing, plain language; must work on low-cost Android phones and slow connections.
- Tamil and Sinhala wording should be reviewed by native speakers.

## Testing
Open `index.html` with Live Server. Test searches that must work:
"Mata painter kenek oni", "ගෙයක් paint karanna kenek oni", "ගෙයක් පින්තාරු කරගන්න කෙනෙක් ඕන", "ගෙයක් හදන්න බාස් කෙනෙක්", "AC එක හදන්න කෙනෙක්", "ගෙදර වයරින් වැඩ කරන කෙනෙක්", "කඩුවෙල පැත්තේ පින්තාරු කරන කෙනෙක්".
