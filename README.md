# Engineer Pay Log — Deadhead Finder Sandbox v3.6.0

## Generalized Physical Finish Resolver

v3.5 proved that **physical finish** and **contractual release** have to be separate clocks. v3.6 moves that logic out of one-off Job 7 / Job 166 exceptions and into a data-driven final-movement resolver.

### What is now automatic
The build contains **117 source-matched weekday/weekend final-movement patterns** extracted from the GO 202 Crew Book. A pattern is included only when its Crew Book report/release exactly matches the already-certified schedule data embedded in the sandbox and the service section has one unambiguous release.

The resolver currently promotes two conservative patterns:

1. **Normal passenger-station finish** — an unmarked final working train ending at Long Island City, Grand Central, Penn Station, Atlantic Terminal, or Jamaica. It is promoted when either:
   - contractual release is exactly 5 minutes after final arrival, or
   - the Crew Book shows a `DH to ...` after the final working movement.

   Physical finish = final train arrival. Paper home availability = physical finish + 5 minutes.

2. **Final `w` movement into Penn** — `w` means the train terminates in West Side Yard.

   Physical finish = Penn arrival + 15 minutes (5 minutes clear + 10 minutes yarding). Paper home availability at Penn = physical finish + 20 minutes. WSY practical-intercept candidates are now measured from **physical finish**, not automatically from contractual release.

### Intentionally not auto-resolved yet
Final `c`, `t`, `v`, and `y` equipment markers remain excluded from the automatic physical-finish layer. Those movements terminate in Penn C Yard, GCM Tail Track, Flatbush Avenue VD Yard, and Jamaica Yard respectively and need their own egress/disposal handling before Deadhead Finder should route from them.

Sections with multiple day-specific release variants are also skipped rather than guessed.

### Existing behavior retained
- Job 7 LIC handling and curated LIC/Hunterspoint originating trains.
- West Side Yard practical vs protected routing.
- Jamaica Storage Yard / Bolands practical layer.
- GTFS browser cache.
- Relief-crew day resolution.
- Contractual report/release remain independent from physical finish.

## Targeted tests

### Test 1 — Job 104, Monday
Expected:
- Final working Train **1591 w** arrives Penn **12:18 AM**.
- Physical finish resolves to **West Side Yard at 12:33 AM**.
- Contractual release remains **12:33 AM**.
- Paper legal at Penn becomes **12:53 AM**.
- Existing WSY practical route behavior remains intact.

This also exercises **Relief Crew 435 Monday**, because 435 covers Job 104 on Monday.

### Test 2 — Job 89, weekday
Expected:
- Final working Train **1235** arrives **Grand Central at 8:50 AM**.
- Crew Book then shows a deadhead to Penn.
- Physical finish resolves to **Grand Central at 8:50 AM**.
- Paper-legal home availability begins **8:55 AM**.
- Contractual Crew Book release remains **9:50 AM** for pay/contract purposes.
- Going-home routing starts from Grand Central, not West Side Yard/Penn.

### Regression
Job 7 Monday should remain LIC **2:56 PM**, paper legal **3:01 PM**, contractual release **4:06 PM**. Job 166 should continue to route correctly, but now through the generalized normal-station rule rather than dedicated hard-coded movement data.
