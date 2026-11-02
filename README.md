# Chess Learning App

A pastel, kid-friendly chess learning web app for iPad. Built as a single static
file (`index.html`) backed by Firebase Realtime Database, so progress syncs
across devices with no login required on the kids' iPads.

- 15 lessons with quizzes, animated demonstrations and voice narration
- Adaptive puzzles, vs-computer play (5 levels), live cross-device family games
- Parent dashboard (PIN-gated) with per-child progress, plus a grown-up play mode
- Stars, badges, streaks and coaching notes

## Revisions

### Rev 10 — 2026-11-01
- **Tic-Tac-Toe** added as its own home-screen section (chess untouched):
  vs the computer at three levels (Easy = random, Medium = blocks and takes
  wins, Hard = perfect minimax play that can never lose — verified
  exhaustively), live family games with create/join-by-code plus the same
  in-progress games list and resume support as chess, rematch with swapped
  symbols, and pass-and-play. Live games live under `tttGames/` in the same
  family tree (existing rules cover it); results are logged as activities,
  not into the chess games history.

### Rev 9 — 2026-11-01
- New "next level" learning phase (the 15 lessons are untouched): **Famous
  games explained** — three complete real games (Morphy's Opera Game 1858,
  Scholar's Mate, Légal's Mate 1750) replayed move by move with a plain-words
  explanation of every move by both sides, ⭐ key-move moments, optional voice
  narration (same toggle as lessons), step/autoplay controls, and a "What to
  remember" strategy card at the end. Watching a game to the end earns +10 XP
  (once per game) and is remembered on the list.

### Rev 8 — 2026-10-31
- Live family games can now be rejoined after a disconnect, dead battery, or
  closed app. The Play family screen lists "Your games in progress" (opponent,
  move count, whose turn, last played) with a Continue button, entering a
  game's code again also resumes it if you're one of the players, and a stuck
  game can be removed from the list (gone for both players, nobody wins).
  Game state was already stored server-side; this adds the way back in.

### Rev 7 — 2026-10-20
- Bug fix: the Undo button in vs-computer games stayed greyed out for the whole
  game because its enabled state was only set when the screen first drew. It
  now refreshes after every move.

### Rev 6 — 2026-10-19
- Vs-computer games now have an Undo button: take back the last full round
  (your move + the computer's reply), up to 3 times per game. Captures,
  castling rights and the move list are all restored correctly.

### Rev 5 — 2026-10-18
- Resigning a game now asks for confirmation first, in both live family games
  and vs-computer games — a mis-tapped button can no longer end a game (and
  record a bogus result) by accident.

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
