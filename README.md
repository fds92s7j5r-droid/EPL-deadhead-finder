# Engineer Pay Log — Deadhead Finder Sandbox v3.2.0

This build rolls the untested v3.1.1 GTFS-cache work together with two routing improvements from the latest review.

## 1. Crew Book-derived West Side Yard intelligence
Instead of manually seeding Train 1902, v3.2 derives WSY train intelligence from the current GO 202 Crew Book:

- `w` + **Lv. Penn Station** = originates in West Side Yard.
- `w` + **Arr. Penn Station** = terminates in West Side Yard.
- 283 unique Penn-origin `w` train numbers were extracted.
- 277 unique Penn-terminating `w` train numbers were extracted for future use.
- Train 102, 802, 1902 and 1904 are all in the derived origin set.

For a current GTFS trip that matches a `w`-origin train, EPL uses the Crew Book Penn clock when it is within five minutes of the GTFS public time, then calculates the WSY Q stop as **Crew Book Penn time − 15 minutes**. This preserves the known one-minute public-vs-Crew-Book difference.

## 2. Dominated-transfer cleanup
Deadhead Finder now suppresses a route that:
- starts at Station A,
- rides another train somewhere else,
- then transfers onto Train B downstream,
- when Train B was already boardable at Station A after the engineer became available.

Example that should disappear:
**Penn → Train 802 → Jamaica → Train 1904**

If Train 1904 is boardable at Penn, EPL should tell the engineer to wait at Penn and board 1904 there.

When equivalent routes arrive at the same time, the router now prefers:
1. fewer train legs,
2. then the later departure from the origin.

## 3. GTFS persistence from v3.1.1
The last successfully loaded GTFS ZIP is cached in IndexedDB and should restore automatically on the next reload when testing from the same GitHub Pages origin.

## Suggested first test
Use **Job 108** again.

For a 12:28 AM WSY release:
- protected Penn availability remains **12:48 AM**;
- Train **1902** should still be recognized as a possible WSY intercept;
- Train **102** should now also be recognized as a WSY-origin candidate when it helps the selected destination;
- a protected Ronkonkoma result should no longer recommend **802 → 1904 at Jamaica** if 1904 can be boarded directly at Penn.

This remains a sandbox and does not modify EPL production.
