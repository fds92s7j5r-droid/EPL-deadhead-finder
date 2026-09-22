# Engineer Pay Log — Deadhead Finder Sandbox v3.5.0

## Physical Finish vs Contractual Release

This build formalizes the distinction discovered while testing Job 7.

### The model
An assignment can have:
- **Contractual home terminal** — where the job is contractually based.
- **Contractual report** — the Crew Book report time.
- **Contractual release** — the Crew Book release used for pay.
- **Physical finish location** — where the engineer actually finishes the final working movement.
- **Physical finish time** — when that train work actually ends.
- **Deadhead availability** — physical finish plus the applicable connection/disposal rule.

A paid home-terminal deadhead/travel allowance can extend the contractual release even though the engineer is not physically required to travel back to the home terminal.

### Job 7 proof case
- Contractual home terminal: **Jamaica Storage Yard**
- Contractual report: **7:43 AM**
- Final working movement: **Train 13**
- Physical finish: **Long Island City, 2:56 PM**
- Paper-legal LIC deadhead availability: **3:01 PM**
- Contractual Crew Book release: **4:06 PM**

Deadhead Finder routes from LIC at the physical-finish clock. Engineer Pay Log/pay logic retains the contractual release separately.

### Data-model cleanup
Movement intelligence now uses explicit fields:
- `physicalFinishTime`
- `physicalFinishLocation`
- `physicalFinishTrain`
- `contractualReleaseTime`
- `contractualHomeTerminal`
- `deadheadPaperMinutes`
- `deadheadAvailabilityBase`

Temporary legacy aliases remain so the existing sandbox router stays stable while the new model is proven.

### Scope
This build does **not** invent final-movement data for assignments we have not certified. Those jobs retain the prior terminal/release behavior until their physical finish is known.

### Retest
Use **Job 7 — Monday Sep. 21 — Ronkonkoma**.

Expected:
1. To-work side still uses JSY / Jamaica −20.
2. Assignment summary explicitly shows **Physical finish: Long Island City · Train 13 · 2:56 PM**.
3. Assignment summary separately shows **Contractual release: 4:06 PM**.
4. Going-home detail explains **3:01 PM paper-legal at LIC** and that the 4:06 release is retained for pay rather than as a physical travel requirement.
5. The route itself should remain the same LIC-based route family that passed in v3.4.1.
