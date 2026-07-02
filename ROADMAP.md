# NUS LiveBus — Roadmap

A living document. Items move top-down: **Now → Done**; things blocked on
external factors sit in **KIV** with a pointer to their write-up.

## Now (this iteration)

| #   | Feature                                                                                                                                  | Area                    | Notes                                                                                                                                              |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Tap a stop on the home map → that stop's page                                                                                            | `HomeMap`               | Applies to both the Stops markers and the Routes-view stop squares.                                                                                |
| 2   | Citymapper-style centre cursor: dragging the home map shows a fixed crosshair in the centre and the drawer lists the stops nearest to it | `HomeMap`, home drawer  | The Nearby list follows the cursor, not just the GPS fix; GPS still recentres the map when granted.                                                |
| 3   | Stop page gets the same map instance — map 20% of the screen, drawer 80%                                                                 | `/stop/[stopName]`      | Reuses the shared map component focused on the stop; other stops remain tappable.                                                                  |
| 4   | Enable-Location banner rides the drawer                                                                                                  | home drawer             | Was capped at 72% viewport height so the expanding drawer slid over it; it now tracks the drawer's top edge (and animates with the same snap).     |
| 5   | Favicon matches the app-icon colour scheme                                                                                               | `static/favicon.svg`    | Off-white `#f9fafd` background, `#446acc` bus — same palette as `512x512.png`.                                                                     |
| 6   | First / Last bus table on every route                                                                                                    | Routes tab              | Weekday / Saturday / Sunday & PH columns, researched from Land Transport Guru + SgWiki (post-Jan-2026 revamp), cross-checked to the minute.        |
| 7   | Starring is a star (not a bookmark) and is instant                                                                                       | stop page, starred page | Star/unstar toggles optimistically; the form action no longer re-runs every load function (which re-fetched live timings upstream — the perf bug). |

## KIV (blocked on data)

| Feature                                                                          | Blocker                                                     | Write-up                                                 |
| -------------------------------------------------------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------- |
| Next live bus per route on the stop-page map (coloured circles in route colours) | No confirmed live GPS/position endpoint on the public proxy | [docs/live-bus-locations.md](docs/live-bus-locations.md) |

## Later / ideas

- Reset the Nearby list's "Show more" depth when the cursor moves far.
- Walking-distance labels on Nearby cards (`formatDistance` already exists in `src/lib/geo.ts`).
- Vacation-period first-bus variants in the schedule table (D1 differs in term vs vacation).
- Outreach to the upstream proxy maintainer before increasing poll rates (`handoff.md` §6).

## Done (before this iteration)

- Live arrivals per stop via the `bus.hewliyang.com` proxy, with streaming SSR + 20 s polling.
- Home map with viewport-lazy stop markers, Stops/Routes views, route lines with direction arrows.
- Stop search, starred stops (cookie-backed), light/dark theme with map reskin.
