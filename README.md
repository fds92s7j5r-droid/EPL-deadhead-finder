# Engineer Pay Log — Deadhead Finder Sandbox v3.1.0

## West Side Yard Terminal Reality Layer

This build models two separate going-home clocks for West Side Yard jobs.

### 1. Protected / paper-legal
- Printed WSY release remains the Crew Book release.
- Legal walking time from West Side Yard back to Penn is **20 minutes**.
- Therefore a protected deadhead from Penn cannot begin before:
  **printed release + 20 minutes**.

### 2. Practical / real-world WSY intercept
- EPL may surface a train that is known to **originate in West Side Yard**.
- Its timetable Q stop is modeled as **15 minutes before the public Penn departure**.
- If that Q stop falls from **15 minutes before the printed release up to 20 minutes after release**, EPL can show it as:
  **POSSIBLE WSY INTERCEPT — NOT PROTECTED**.
- This does not mean the train will wait. It is only a possible real-world connection.
- A Community Deadhead Note can be attached to the route. In the future, repeated community confirmations can increase confidence without ever changing the paper-legal calculation.

### Seeded research train
- **Train 1902** is currently entered as a known WSY-origin train so the concept can be tested.
- More WSY-origin trains should be added only when their origin/Q-stop relationship is certified.

### Important
- Protected and practical routing are intentionally separate.
- A practical WSY intercept never becomes “legal” because of a note.
- Other yard egress rules (JSY, Babylon Yard, Ronkonkoma Yard, Hillside) remain paused until certified.
- Manual GTFS upload and all v3.0.4 routing behavior remain in place.
