# Chess Learning App

A pastel, kid-friendly chess learning web app for iPad. Built as a single static
file (`index.html`) backed by Firebase Realtime Database, so progress syncs
across devices with no login required on the kids' iPads.

- 15 lessons with quizzes, animated demonstrations and voice narration
- Adaptive puzzles, vs-computer play (5 levels), live cross-device family games
- Parent dashboard (PIN-gated) with per-child progress, plus a grown-up play mode
- Stars, badges, streaks and coaching notes

## Revisions

### Rev 4 — 2026-10-17
- Parent play mode: grown-ups get their own lessons, puzzles, vs-computer games
  and family games under their own profile via "Enter play mode" on the parent
  dashboard. Progress is tracked separately; the PIN still gates the dashboard.
- The parent dashboard's per-child detail now shows the child's age.
- "＋ Add a player" is now prominent on the Who-is-playing screen; fixed a bug
  where entering an adult age created a kid profile instead of a grown-up one.
- First revision deployed directly via the GitHub API (no manual upload).

### Rev 3 — 2026-10-12
- Replaced Unicode chess glyphs with inlined Cburnett SVG piece artwork
  (CC BY-SA 3.0, via Wikimedia Commons): white pieces now render white with
  dark outlines, clearly visible on every square color, with a soft drop shadow.
- Richer board: gradient squares and a deeper frame shadow.
- Captured-piece trays now show true piece colors.

### Rev 2 — 2026-10-12
- Board cells forced square via per-square aspect-ratio.
- Animated demo steps: pieces slide with synchronized narration.
- Narration via iPad speechSynthesis (prefers Siri/natural voices) with a
  speaker toggle; lesson steps and quiz questions are read aloud.
- New lessons 13–15: Trickier Tactics (skewers, discovered attacks, double
  checks), Checkmate Patterns (back-rank mate, ladder mate), Think Like a
  Champion (development, center control, outposts, king safety).
- Lesson totals are now dynamic; the original 12 lesson IDs are unchanged, so
  existing progress was preserved.

### Rev 1 — 2026-10-11
- Initial release: 12 lessons with quizzes, adaptive puzzles, vs-computer play
  (5 fun levels), live cross-iPad family games, parent dashboard with PIN,
  stars and badges, Firebase Realtime Database backend.
- chess.js 0.12.0 UMD inlined, so the rules engine has zero CDN dependency.
