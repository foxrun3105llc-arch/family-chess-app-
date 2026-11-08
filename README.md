# Chess Learning App

A pastel, kid-friendly chess learning web app for iPad. Built as a single static
file (`index.html`) backed by Firebase Realtime Database, so progress syncs
across devices with no login required on the kids' iPads.

- 15 lessons with quizzes, animated demonstrations and voice narration
- Adaptive puzzles, vs-computer play (5 levels), live cross-device family games
- Parent dashboard (PIN-gated) with per-child progress, plus a grown-up play mode
- Stars, badges, streaks and coaching notes

## Revisions

### Rev 15 — 2026-11-07 — Grounded pieces, Reset view, 2D/3D toggle
- Bug fix: pieces on the 3D stage floated about a row above their squares. The piece <svg> has no
  viewBox of its own, so height:auto gave it a tall letterboxed box and the artwork centered inside it.
  Stage pieces now force a square aspect, and the anchor moved from 50% to 43% so the drawn base of the
  Merida artwork (which ends ~93% down its canvas) sits exactly on the contact shadow.
- View controls on game boards now include: Reset view (back to the starting tilt 52 / angle 0), and a
  2D/3D toggle — 2D renders the classic flat top-down board; the button then offers 3D, and vice versa.
  The chosen mode is remembered per device along with tilt/angle.
- Service worker cache bumped to v15.


### Rev 14 — 2026-11-07 — Nearby play (hotspot + QR handshake)
- New mode in Play family: two iPads on the same Wi-Fi or phone hotspot play live with NO internet.
  Connection invitation is exchanged by QR code (host shows an offer code, guest scans and shows an
  answer code, host scans back); moves then flow over a WebRTC data channel. No servers, no accounts.
- Signaling payloads are compacted SDP (ICE ufrag/pwd, DTLS fingerprint, host candidates only) —
  about 340 chars, a 65x65-module QR. QR toolkit bundled inline (jsQR, Apache-2.0; qrcode-generator,
  MIT) so scanning/generation also work offline.
- Finished nearby games are recorded like live family games (winner +15 XP, game record) through the
  rev 12 outbox, so results earned offline sync when internet returns. Unfinished games end if the
  connection drops — nothing is stored server-side. Pass-and-play still covers the no-network-at-all case.
- Service worker cache bumped to v14.


### Rev 13 — 2026-11-07 — Merida pieces, 3D tournament boards, jumbo home
- Piece artwork swapped everywhere from Cburnett outlines to the shaded Merida set
  (design: Armando H. Marroquin, via Lichess's open piece library), bundled inline so it works offline.
- Game boards (vs computer, live family, pass-and-play) now render on a 3D stage: cream + tournament-green
  squares (#EEEED2 / #769656 family) on a dark walnut slab, player-perspective view, pieces standing and
  anchored to their square centers with silhouette + contact shadows.
- Tilt (0-65) and Angle (-60..60) sliders under each game board; the chosen view is remembered per device.
- Board coordinates (a-h, 1-8) enlarged and bolded on the edges of every board.
- Home screen rebuilt as option B jumbo tiles: Next lesson, Puzzles, Play the computer, Play family,
  Famous games, Tic-Tac-Toe.
- Lessons, puzzles and demo boards stay flat for readability. Service worker cache bumped to v13.


### Rev 12 — 2026-11-07
- **Offline mode:** the app now works with no internet (road trips!). Each iPad keeps a saved copy of the family, profiles, and progress, so the app opens and plays fully offline — lessons, puzzles, vs-computer, famous games, tic-tac-toe, and pass-and-play. A small "📴 Offline" pill shows when there's no connection.
- **Syncs itself later:** everything done offline (game results, XP, lessons, streaks) is stored on the iPad in an outbox and uploads automatically when the internet comes back, with a "☁️ Synced!" note when it lands.
- **Pass-and-play counts now:** before a pass-and-play game you pick who is White and who is Black from the family profiles (or Guest for a just-for-fun game). With two named players, the result is saved for both — winner gets +15 XP, a draw gives both +5 — online or offline. The end screen keeps the final board visible with the winner's name.
- Live two-iPad games still need the internet; the Play family screen says so when offline and points to pass-and-play instead.
- Added a service worker so the app itself can open offline after the first online visit.


### Rev 11 — 2026-11-01
- Game-over screens (vs computer and live family games) now keep the final
  board on screen under the result banner, with the last move and the
  checkmated king highlighted — so kids can study the mating position instead
  of it vanishing the moment the game ends. Checkmate finishes add a line
  explaining the king has no escape.

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
