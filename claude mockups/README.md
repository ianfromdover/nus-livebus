# D2 live bus marker — icon brainstorm (4 options)

Mockups for the icon that shows the **current location of a D2 bus** on the map.
All options use the app's existing D2 route colour `#6E1D72` / white (`src/lib/routes.ts`).
The bus glyph is Material Symbols `directions_bus` (Apache 2.0).

| File | Option | Pattern | Inspired by |
|---|---|---|---|
| `option-1-d2-marker.png` | 1 — Live Puck | White ringed circle + bus glyph + heading arrow + D2 badge | Google Maps live transit puck; Transit app GPS marker |
| `option-2-d2-marker.png` | 2 — Route Chip | Floating "bus + D2" pill anchored to a GPS dot | Moovit live-vehicle labels; Citymapper route pills |
| `option-3-d2-marker.png` | 3 — Teardrop Pin | Classic pin, bus glyph in head, D2 corner badge | Transit app vehicle pins; classic map-pin convention |
| `option-4-d2-marker.png` | 4 — Direction Puck | Solid D2 disc + rotating travel wedge + pulse halo | OneBusAway heading markers; navigation-app chevron pucks |

`00-overview-all-options.png` shows all four side by side at map scale.
`src/` contains the HTML/SVG used to render the mockups — the marker SVGs can be
lifted straight into a MapLibre/Leaflet marker.

## Reference sources

- Transit app — live vehicle tracking on map: https://transitapp.com/ and https://help.transitapp.com/article/93-how-to-use-transit
- Moovit — "Live Location: see your line in real-time on a map": https://support.moovitapp.com/hc/en-us/articles/12708869559826-Live-Location-See-Your-Line-in-Real-Time-on-a-map
- OneBusAway — bus position marker design discussion (heading wedge): https://github.com/OneBusAway/onebusaway-android/issues/176
- BusWhere — bus tracking app UI guide (color-coded live markers): https://www.buswhere.com/bus-tracking-app-ui/
- Google Maps transit symbols explained: https://techpp.com/2025/03/27/google-maps-symbols-and-icons-explained/
- Fuselab — transportation app UI/UX best practices: https://fuselabcreative.com/transportation-app-ui-ux-design-best-practices/
- Dribbble transit-app tag (visual inspiration): https://dribbble.com/tags/transit-app
- Figma CityTrans bus app UI kit: https://www.figma.com/community/file/1403960124614816925/citytrans-bus-transportation-app-ui-kit
