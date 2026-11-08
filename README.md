# Chess Learning App

A pastel, kid-friendly chess learning web app for iPad. Built as a single static
file (`index.html`) backed by Firebase Realtime Database, so progress syncs
across devices with no login required on the kids' iPads.

- 15 lessons with quizzes, animated demonstrations and voice narration
- Adaptive puzzles, vs-computer play (5 levels), live cross-device family games
- Parent dashboard (PIN-gated) with per-child progress, plus a grown-up play mode
- Stars, badges, streaks and coaching notes

## Revisions

### Rev 20 — 2026-11-08 — 3D coordinate polish (from live browser QA)
- A cloud-browser agent played the live app as Ria and verified rev 19 end to end: real 3D
  pieces with shading/shadows, all 32 pieces placed, sliders and Reset view working, and a
  full pawn move (b2-b4, computer replied Nc6) completed by tapping in 3D. No errors.
- Its one nit: in 3D the coordinates sat in the corners of the edge squares — the h-file
  letter was hidden behind the h1 rook and the rank numbers were small/faint. Coordinates
  now ride on the wooden slab edge outside the squares (cream on walnut, slightly larger),
  so no piece can ever cover them.

### Rev 19 — 2026-11-08 — Real 3D board (WebGL), toggle with 2D
- The "3D view" is now REAL 3D: Three.js r149 (MIT) is inlined (no CDN, offline still works)
  and pieces are true 3D Staunton-style models built procedurally in code — lathe-turned
  pawn/rook/bishop/queen/king bodies, an extruded knight, glossy materials, real lights and
  shadows on the cream + tournament-green board. No external model files or licences needed.
- The old CSS billboard stage is retired (flat pictures standing up); in 3D mode drawBoard
  routes game boards (vs computer, live, nearby, pass-and-play) to the WebGL renderer.
  Board taps are raycast onto per-square hit planes; highlights, move dots, capture rings and
  a-h/1-8 coordinates (sprites) all work in 3D. Tilt/Angle sliders and Reset view now drive
  the camera. Camera elevation is clamped to 88 degrees (a perfectly vertical view makes
  the picking math degenerate).
- 2D is unchanged and remains the default (rev 18). The toggle works both ways mid-game.
  If WebGL is unavailable or fails, the board falls back to the flat 2D renderer.
- Verified in Node (no WebGL available headless here): full-app syntax check; a 101-assertion
  suite — piece bounding boxes (bases at y=0, heights pawn .68 < rook .75 < knight .97 <
  bishop .98 < queen 1.02 < king 1.12, all fit their square) and tap-picking: projecting all
  sampled squares back through the camera raycasts to the correct square at tilts 0/35/52/65,
  spins -60/+60, for both White and Black orientations, with correct screen corners.
  On-device visual check is still the real test — iPads open the app online once to update.

### Rev 18 — 2026-11-08 — Flat 2D board is the default again
- Kid feedback on the rev 13 3D stage: the tilted board is hard to read (hard to tell which
  square a piece is on) and the standing pieces "look like images rotating instead of objects"
  — accurate: they are flat artwork billboards counter-rotated to stay upright, so they can
  never look like real 3D objects. True 3D pieces would need a WebGL rebuild.
- Game boards (vs computer, live, nearby, pass-and-play) now open FLAT by default. The view
  preference storage key moved to cla_view3d_v2, so every device resets to flat once and old
  saved tilt/angle choices are ignored; the 3D view is still one tap away in the view controls
  ("Switch to 3D view") and is remembered under the new key if chosen.
- Flat game boards use the cream + tournament-green squares from the 3D board (instead of the
  wood-brown), so the colours stay consistent and high-contrast. Lessons, puzzles, demos and
  final boards are unchanged.

### Rev 17 — 2026-11-07 — Fair draw offers (vs computer)
- The "Draw?" button is now a real draw offer. The computer accepts only when both are true:
  (1) at least 20 moves each have been played, and (2) the computer is not ahead by more than
  about a pawn on the same evaluation the AI plays with.
- Declines explain why in kid-friendly words ("Too early!" / "I'm a little ahead — let's keep
  playing!") and the game continues; after a decline, 5 moves each must be played before asking again.
- Why: several recorded "draws" were far too short to be genuine board draws (e.g. a level-3 draw
  after 6 moves each) — the old button ended the game as a draw on one tap, with no agreement.
- Every vs-computer game record now also stores how it ended (endReason: checkmate, stalemate,
  threefold-repetition, insufficient-material, fifty-move, agreed-draw, resign) plus the final
  position (FEN), so future results can be verified.

### Rev 16 — 2026-11-07 — Rock Paper Scissors (live family)
- New home tile and hub: Rock Paper Scissors sessions between two iPads (create/join by 4-letter
  code, sessions in progress listed with resume/remove, same patterns as the other family games).
- Each round: 5-second pick timer with countdown bar. No pick loses the round; two no-picks is a
  draw. Picks stay secret until both are locked (or time expires).
- Reveal: both fists shake three times ("Rock… Paper… Scissors…") then land on the actual gestures
  with a pop; winner glows, loser dims; a timeout shows a clock and "didn't pick!".
- Session scoreboard (wins · draws · wins, round number) persists in the Firebase game node.
  After each round BOTH players must press Next to start the next round. Ending the session shows
  the final tally; the session winner gets +10 XP and one session record is saved.
- Round resolution and round advance use Firebase transactions, so simultaneous triggers from
  both iPads can never double-count a score (verified by a 19-assertion Node simulation).
- Service worker cache bumped to v16.


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
