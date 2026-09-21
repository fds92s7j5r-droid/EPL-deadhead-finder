# Engineer Pay Log — Deadhead Finder Sandbox v3.3.0

## Jamaica Storage Yard + Bolands Landing test build

### Protected JSY rule
- To work: must reach Jamaica 20 minutes before JSY report.
- Going home: paper-legal at Jamaica 20 minutes after printed JSY release.
- Bolands Landing never changes the written legal calculation.

### Practical JSY-origin candidates
EPL may surface a **likely JSY-origin** diesel train when:
- its GTFS trip begins at Jamaica,
- it is in the LIRR diesel passenger train-number families (1–99, 500–699),
- and its scheduled Jamaica departure is within ±10 minutes of printed JSY release.

This is an inference, not a Crew Book origin marker, so the route remains NOT PROTECTED.

### Bolands Landing
Default walking estimate: **~7 minutes from JSY**.

Timing hierarchy:
1. Curated scheduled Bolands E-stop
2. East New York + 5 minutes
3. Atlantic Terminal + 16 minutes when ENY is skipped

Curated weekday E-stops:
- 2800 — 12:18 AM
- 1710 — 7:53 AM
- 2904 — 3:15 PM
- 2906 — 3:35 PM
- 1756 — 3:58 PM
- 2910 — 4:17 PM
- 2918 — 4:51 PM
- 772 — 6:55 PM

A scheduled E-stop is operationally certain (the train must stop), but using Bolands for JSY access/egress is still not protected because the written rule is Jamaica ±20 only.

### Suggested tiny test
1. **Job 3, Monday Sep. 21** — confirm protected going-home routing does not begin before **9:41 AM Jamaica** after the 9:21 release.
2. **Job 7, Monday Sep. 21** — look for the **1756 scheduled Bolands E-stop at 3:58 PM** around the 4:06 PM release. EPL should explicitly show the ~7-minute walk and that this would require being physically clear before the printed release.

The v3.2.1 GTFS IndexedDB cache behavior is unchanged.
