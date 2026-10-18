# 🕵️ IMPOSTER

A real-time, multiplayer **word party game** for 3–20 players — the impostor game,
now a installable PWA.

Everyone gets the **same secret word**… except one or more players, who secretly
get a **different word from the same category**. Discuss together, argue about
who doesn't belong, and vote them out. If an impostor survives the vote, they get
one guess at the real word to steal the win.

## Run it

```bash
npm install   # just the `ws` WebSocket library
npm start     # serves the game on http://localhost:5173
```

Then open `http://localhost:5173`. Two ways to play:

- **Multi-device** — create a room, share the **4-letter room code**. Friends on
  the same Wi-Fi open the LAN address shown in the lobby (e.g. `http://10.200.34.43:5173`)
  and enter the code.
- **Pass & Play** — one phone passed around the table. Add 3–20 names; the game
  hands the device to each player in turn ("look away" screens between turns).

```bash
npm test      # end-to-end test: plays full rounds over real WebSockets
```

Set `PORT` to serve on a different port.

## Round flow

1. **Deal** — crew all get the same word; each impostor gets a different one from
   the same category (press & hold to peek at your card).
2. **Discuss** — talk it out in person (or in chat).
3. **Vote** — one vote each (the host can close voting early). Chat stays open
   while you decide. Most votes is eliminated;
   ties and skips mean nobody is.
4. **Last guess** *(house rule, on by default)* — an impostor who is voted out gets
   one shot at the real word. Correct guess steals the win.
5. **Reveal & score** — roles, the word, and a running scoreboard. Play again keeps
   the room together.

## Host controls & settings

Game mode (**Classic** — impostor knows nothing, **Mysterious** — impostor gets a
similar decoy word, **Chaotic** — impostor count is random and hidden), category
(including a host-supplied **custom word list**), impostor count,
last-guess rule, and room size (6/12/20). The host
can also kick players, transfer host, and force-start with fewer players.

## Layout

- [server.js](server.js) — HTTP + WebSocket server, rooms, and the game state machine
  (all secrets stay server-side)
- [public/index.html](public/index.html), [public/app.js](public/app.js),
  [public/style.css](public/style.css) — the responsive client (light theme, 48px
  touch targets, screen-reader announcements)
- [public/manifest.webmanifest](public/manifest.webmanifest), [public/sw.js](public/sw.js),
  [scripts/make-icons.mjs](scripts/make-icons.mjs) — PWA install + offline shell
- [test/e2e.mjs](test/e2e.mjs) — plays complete rounds and asserts the whole flow

Rooms live in memory: restart the server and the rooms are gone (players are
offered the home screen and can create or join again).
