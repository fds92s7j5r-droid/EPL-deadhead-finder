# Engineer Pay Log — Deadhead Finder Sandbox v3.4.1

## LIC detail-card hotfix

The v3.4.0 screenshots exposed a UI/runtime bug rather than a bad Home Origin calculation.

What happened:
- Job 7 correctly resolved its home origin to **Long Island City**.
- The priority comparison correctly calculated LIC-based routes.
- There was no under-5-minute practical LIC departure for the selected case.
- v3.4.0 still tried to classify that nonexistent practical route, causing a JavaScript exception.
- Because the exception occurred after the LIC header and priority comparison were already updated, the old detailed route card from the previous render remained visible. That stale card was the bizarre Grand Central / Sunday itinerary.

v3.4.1 fixes this by:
1. Handling a null practical LIC route safely.
2. Clearing the previous detailed home card before recalculating so stale data cannot remain after an error.
3. Leaving the Home Origin Resolver and LIC routing logic unchanged.

### Retest
Use **Job 7 — Monday Sep. 21 — Ronkonkoma**.

Expected:
- Report side remains JSY / Jamaica −20.
- Home origin remains LIC after Train 13 at 2:56 PM.
- Going-home detail should now show the same LIC-based route family as the priority comparison.
- No Grand Central/Sunday stale route should appear.
