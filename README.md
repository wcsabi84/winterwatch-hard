# Winter Watch Hard

Educational web prototype for County Cavan winter-road decisions.

**Live site:** https://wcsabi84.github.io/winterwatch-hard/

## Features
- **Part 1:** User-provided Gemini API key stored in browser session storage; AI question box and key controls.
- **Part 2:** Ten approximate representative route GPS points and direct Google Maps links. These are practice estimates, not independently surveyed midpoints.
- **Part 3:** Hourly Open-Meteo forecast for tonight; route-by-route deterministic treatment rules, depot resources, departure suggestions, and an AI explanation of the ten calculated results. Duty Engineer retains final authority.
- **Part 4:** Five-winter real historical weather from Open-Meteo, joined to 7,560 operational records by route and evening date; ice investigation and downloadable joined CSV.

## Part 4 verification

| Measure | Result |
|---|---:|
| Operational rows | 7,560 |
| Gritting runs | 439 |
| Salt used | 1,152.45 t |
| Matched weather records | 7,560 |
| Unmatched | 0 |
| Frost route-nights using Open-Meteo 0–7 cm soil temperatures | **472** |
| Lecturer's approximate frost benchmark | **~770** (does not match) |

Most recorded ice incidents on treated nights: **R05 (10)**. On untreated nights: **R10 (19)**.

**Explanation of discrepancy:** Our real archive series uses Open-Meteo modelled 0–7 cm soil temperatures at approximate route locations, whereas the reference ~770 count uses different weather data or definitions; consequently, the frost totals are not directly interchangeable. The separate prepared `history.csv` counts 767 frost route-nights, but that dataset is not substituted for real archived weather in the Hard exercise.

For complete methodology, route-level results, and reproducibility, see [Part 4 verification](part4-verification.md).

## Use and safety
Open the live page; set an AI key privately if you want AI responses; run Part 3 calculations for current forecasts. For Part 4, select the lecturer's `ops.csv` via the file picker if its direct fetch is blocked, click **Build five-winter history + join**, and optionally download the result or use **Ask AI about historic ice**. Never commit API keys to GitHub. Forecasts and soil temperatures are model estimates, not measurements of the road surface, and the site is not a dispatch system.
