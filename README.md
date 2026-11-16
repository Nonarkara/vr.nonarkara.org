# vr.nonarkara.org — Universe 69, the West World

A public snapshot of **Universe 69**: two sealed pixel universes —
Calhoun's Universe 25 (EDEN) and the first-sapiens world (LAGOON) —
running on the Observers' own instrument. This site is the viewing room:
people can visit the West World (VR) and the terrarium (2D) without
touching the instrument.

## What is here

- `vr.html` — the West World. WebXR where a headset answers; drag to
  look, WASD to walk, `r` inspect, `t` table, `1`/`2` the two worlds.
- `2d.html` — the 2D terrarium viewer with the book of the record.
  (`web/index.html`, shipped under its deployed name.)
- `snapshot.json` — the record at the moment of the snapshot. Both
  viewers read it through `config.js` (`window.SNAPSHOT_URL`), the same
  hook the live mirror uses; the pages are byte-identical copies of the
  shipped `web/` sources.
- `CNAME` — `vr.nonarkara.org`, served by GitHub Pages.

## Honesty

- The snapshot is frozen at deploy time. The record counter on the page
  keeps counting — that is how you know it is a snapshot, not the live
  wire. The live instrument stays on the Observers' machine.
- No trajectory was moved by this deploy. Viewer and ops only.
- The worlds are deterministic in code, not in this page: a fresh
  snapshot shows a world that has kept living.

## Refresh the snapshot (owner)

A snapshot without the `/world` payload renders the lagoon black
(fixed 2026-11-15) — so merge state and world, and headless-check the
pages on a scratch static server before pushing:

    curl -s http://127.0.0.1:6969/state > /tmp/state.json
    curl -s http://127.0.0.1:6969/world > /tmp/world.json
    python3 - <<'PY'
    import json
    s = json.load(open('/tmp/state.json'))
    s['world'] = json.load(open('/tmp/world.json'))
    open('snapshot.json', 'w').write(json.dumps(s, separators=(',', ':')))
    PY
    git add snapshot.json && git commit -m "snapshot: the record at <date>" && git push

## DNS (owner)

Point `vr.nonarkara.org` at GitHub Pages — A records
185.199.108.153 / 185.199.109.153 / 185.199.110.153 / 185.199.111.153,
or CNAME to `nonarkara.github.io`. The `CNAME` file is already in the
repo; GitHub provisions the certificate once DNS resolves. If the
domain sits behind a Cloudflare proxy, set it to DNS-only (grey cloud)
— an orange cloud kept the old hub serving the domain after this repo
was already live on Pages.
