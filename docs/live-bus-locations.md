# KIV — Live bus locations on the stop-page map

> Status: **KIV (not implemented)** — blocked on a data source, see
> [Outstanding investigation](#outstanding-investigation). Do not build the UI
> until a live-position endpoint is confirmed; everything else on the stop page
> is already shaped so this can slot in.

## What the feature is

On a bus stop's individual page (`/stop/[stopName]`), the map at the top of the
screen should show **the next incoming live bus for that stop, for each route
that serves the stop** — one moving marker per route, rendered as a filled
circle in that route's colour (`routeColor()` from `src/lib/routes.ts`, white
stroke, same treatment as the user-location dot in `HomeMap.svelte`).

Concretely, for a stop served by A1, D2 and K:

- three circles on the map — red (A1), purple (D2), blue (K) — each at the
  current GPS position of the nearest bus that has not yet reached this stop;
- circles update on the same 20 s poll cadence as the arrival timings (or
  faster if the upstream allows), animating between fixes with a short
  ease so movement reads as travel rather than teleporting;
- tapping a circle is out of scope for v1 (no popup/route link needed);
- buses beyond the "next arrival" for a route are not shown — one circle per
  route, maximum.

## How it should be built (when unblocked)

- **Rendering:** add a `live-buses` GeoJSON source + `circle` layer to the
  shared map component used by the stop page (the `HomeMap.svelte` instance in
  stop-focus mode). Feature properties carry `route`; paint uses
  `['get', 'color']` with the colour resolved through `toHex(routeColor(route))`
  exactly like `routeLineFC` does in `src/lib/mapkit.ts`.
- **Data flow:** the stop page already polls `/api/stop/[code]` every 20 s.
  Extend that endpoint (or add `/api/route/[code]/buses`) server-side so the
  client keeps a single polling loop. Positions for routes serving the stop
  get filtered server-side to the single next-arriving vehicle per route
  (match on `arrivalTime_veh_plate` from the timings payload if the position
  feed exposes plates).
- **Politeness:** the upstream is a personal, unfunded proxy
  (`bus.hewliyang.com` — see `handoff.md`). Poll positions only while a stop
  page is visible, reuse the existing `document.hidden` pause, and cache
  server-side with a short TTL so N viewers of the same stop cost one upstream
  call.

## Outstanding investigation

No live bus GPS/position endpoint has been confirmed yet (`handoff.md` §2):

1. Open `bus.hewliyang.com`'s route pages with the DevTools Network tab and
   look for a `/route/{code}/__data.json` (or similar) response carrying
   `lat`/`lng`/`direction`/`speed` per vehicle. The site's source is at
   `github.com/hewliyang/nus-nextbus-web` — check its `src/routes` tree for an
   `ActiveBus` call.
2. The underlying NUS NextBus API is known to have an `ActiveBus?route_code=X`
   style endpoint, but it sits behind Basic Auth on `nnextbus.nus.edu.sg` —
   reverse-engineering NUS's protected backend is explicitly out of scope; only
   use it if the public proxy resolves it for us.
3. If a position feed exists, confirm it includes the vehicle plate so the
   "next bus" circle can be matched to the `arrivalTime_veh_plate` already
   shown on the arrival rows.
4. Etiquette before shipping: message the proxy's maintainer (hewliyang) —
   position polling is chattier than timings polling.

## Why it's KIV'd

The UI is cheap; the data isn't there yet. Shipping a marker driven by
interpolated arrival-times (rather than real GPS) would show buses gliding
smoothly through places they aren't — worse than nothing for a "live" map.
