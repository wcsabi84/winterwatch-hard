# Winter Watch Hard — Part 4 verification

## Task
Join five winters of hourly historical weather to the supplied operational log by `(route_id, date)`. Each operational date represents a night beginning at 18:00 and ending at 09:00 the following morning (16 hourly values, inclusive). Summarise route nights, frost nights, gritting runs, salt, ice on treated and untreated nights, and runs on non-frost nights.

## Sources and definitions
- Operations: lecturer's `ops.csv`, 7,560 route-date rows for the five winters 2020/21–2024/25.
- Route coordinates: ten provisional representative points documented in the website.
- Weather: Open-Meteo Historical Weather API, retrieved for each route and winter; the joined output contains minimum air temperature, minimum 0–7 cm **soil** temperature, minimum dew point, total **precipitation** (rain and snow combined), and hours with soil temperature at or below 0 °C.
- Frost night: `min_ground_c <= 0`, using the modelled 0–7 cm soil-temperature measurement, *not* road surface temperature.
- Join key: `(route_id, date)`. The date is the **evening** on which the overnight period begins.

## Independent checks of downloaded joined CSV

| Check | Result |
|---|---:|
| Operations / joined records | 7,560 |
| Distinct `(route_id, date)` combinations | 7,560 |
| Duplicate keys | 0 |
| Unmatched operational records | 0 |
| Missing historical weather values in five summary fields | 0 |
| Hours represented per record | 16 |
| Gritting runs | 439 |
| Total salt | 1,152.45 tonnes (1,152.5 t to one decimal) |
| Recorded ice incidents | 40 |
| Frost route-nights from real Open-Meteo join | **472** |
| Lecturer's approximate benchmark | **~770** |

The join preserves the original operational values (`date`, `route_id`, `gritted`, `salt_t`, `ice_incidents`) exactly.

## Route-by-route results

| Route | Nights | Frost nights | Gritting runs | Salt (t) | Ice treated | Ice untreated | Runs with no frost |
|---|---:|---:|---:|---:|---:|---:|---:|
| R01 N3 Cavan Bypass | 756 | 51 | 38 | 79.80 | 0 | 0 | 20 |
| R02 N54 Butlersbridge Road | 756 | 51 | 41 | 55.35 | 0 | 1 | 23 |
| R03 R212 Belturbet Road | 756 | 50 | 34 | 81.60 | 1 | 1 | 16 |
| R04 R154 Kilnaleck Uplands | 756 | 36 | 89 | 293.70 | 0 | 1 | 62 |
| R05 R165 Bailieborough Road | 756 | 49 | 49 | 132.30 | 10 | 1 | 30 |
| R06 Lough Gowna Lake Road | 756 | 42 | 19 | 34.20 | 1 | 0 | 6 |
| R07 R188 Cootehill Road | 756 | 51 | 45 | 135.00 | 1 | 0 | 24 |
| R08 N87 Ballyconnell Road | 756 | 51 | 43 | 109.65 | 2 | 0 | 24 |
| R09 N3 Virginia South | 756 | 47 | 54 | 170.10 | 0 | 2 | 34 |
| R10 R162 Kingscourt Hill | 756 | 44 | 27 | 60.75 | 0 | 19 | 17 |

## The two ice questions
- **Most ice incidents on treated nights:** R05 — R165 Bailieborough Road, **10** incidents.
- **Most ice incidents on untreated nights:** R10 — R162 Kingscourt Hill, **19** incidents.
- Incident-night weather details (soil temperature, precipitation and dew point) are shown by the site's **Ice incident investigation** and supplied to its **Ask AI about historic ice** workflow. These are archive-model values, not actual road-sensor observations.

## Explanation of the frost-count discrepancy
**One-sentence explanation:** The real Open-Meteo join counts 472 nights at or below 0 °C using modelled 0–7 cm soil temperatures at our approximate points, whereas the lecturer's ~770 figure reflects different reference weather data or definitions; therefore the counts are not directly interchangeable.

### Additional check: the separate prepared history file
A separate supplied/prepared `history.csv` (used in the Easy exercise) contains **767** route-nights with `min_ground_c <= 0`, close to the reference of ~770. That is a **different pre-prepared weather dataset** and must not be substituted into the Hard-level output while claiming it came from the live historical API. The source difference explains why simply checking the operational join cannot force the frost figures to match. The exact contribution of weather model, depth, location and reference method is not independently established.

The real-weather value of 472 is **outside** a ±20% band around ~770 (616–924). Report the discrepancy honestly; do not adjust the frost threshold, invent missing observations, or describe this benchmark as passed. All other exact operational and join checks pass.

## Reproduce the checks
1. Open the live Winter Watch Hard website and build Part 4, selecting a local `ops.csv` if the browser cannot fetch it.
2. Confirm 7,560 matched rows and 0 unmatched.
3. Download **joined CSV**.
4. Recheck sums and row counts from that downloaded file; inspect **Ice incident investigation**.
5. Use **Ask AI about historic ice** (not the generic Part 1 Ask AI button) and check its answer against the computed incidents.

**Status:** Data retrieval, join, summaries, downloadable evidence, and AI question work. The approximate frost benchmark is *not* met by the chosen real archive series, and its difference is recorded transparently.
