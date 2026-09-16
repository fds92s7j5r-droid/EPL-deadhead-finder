# Engineer Pay Log — Deadhead Finder research pack

Generated: 2026-09-16

## Executive result

The Crew Book is a much stronger source for this project than expected. I parsed the Equipment Train Assignment indexes and matched every indexed movement back to an actual movement on a crew assignment page.

- Equipment index entries: **208**
- Unique train identifiers: **205**
- Monday–Friday eastbound entries: **92**
- Monday–Friday westbound entries: **78**
- Saturday–Sunday entries: **38**
- Indexed equipment entries matched to a Crew Book movement: **208 / 208**

That supports the user's point: if a regularly scheduled train exists, a crew has to operate it, so the Crew Book gives us a discovery source even when the move is invisible to public GTFS.

## Best architecture discovered

1. **GTFS public service layer** — normal passenger trips/stops/times.
2. **Crew Book service layer** — passenger and equipment movements as actually assigned to crews.
3. **Diff detector** — match by train number/day/origin and allow public departure = Crew Book departure or Crew Book departure minus 1 minute. Flag endpoint/stop discrepancies rather than auto-declaring them E Stops.
4. **Employee-service certification layer** — human-reviewed classification: legal E-stop service, Q-stop/non-guaranteed option, not usable, or needs review.
5. **Access-time layer** — walking/yard/terminal rules applied to routing deadlines.

## GTFS snapshot alignment

Transitland identifies a Long Island Rail Road GTFS feed version added 2026-09-09 whose service starts **2026-09-08**, with feed version name **GO202_26**. That is the exact effective date of this Crew Book revision, making it the correct static snapshot to compare against. The snapshot contains 2,076 trips and 21,625 stop-time rows.

## Proof-of-concept E-stop candidate scan

These are **flags for employee timetable review, not certifications**:

| Train | Crew | Crew Book | Public GTFS view | Why it flags |
|---|---:|---|---|---|
| 13 | 7 | Montauk 11:30 AM → Long Island City 2:56 PM | Montauk 11:29 AM → Jamaica 2:29 PM | Public departure is exactly 1 minute earlier (allowed matching rule), but public service terminates at Jamaica while Crew Book continues to Long Island City. |
| 45 | 53 | Speonk 4:10 PM → Hunterspoint Avenue 6:10 PM | Speonk 4:10 PM → Jamaica 5:52 PM | Same advertised departure, but public trip terminates Jamaica while Crew Book continues west to Hunterspoint Avenue. |
| 615 | 32 | Port Jefferson 6:14 AM → Long Island City 8:08 AM | Port Jefferson 6:13 AM → Hunterspoint Avenue 7:59 AM | Public departure is 1 minute earlier (allowed), and public schedule ends at Hunterspoint Avenue while Crew Book continues to Long Island City. |
| 621 | 35 | Port Jefferson 7:32 AM → Long Island City 9:21 AM | Port Jefferson 7:31 AM → Hunterspoint Avenue 9:13 AM | Public departure is 1 minute earlier (allowed), and public schedule ends at Hunterspoint Avenue while Crew Book continues to Long Island City. |
| 8 | 49 | Long Island City 11:18 AM → Montauk 2:26 PM | First public time 11:41 AM → Montauk 2:26 PM (Jamaica is the public origin in current public timetable) | Same final arrival, but Crew Book starts the train 23 minutes earlier at Long Island City. |
| 18 | 10 | Long Island City 4:16 PM → Montauk 7:41 PM | First public time 4:30 PM; Jamaica 4:48 PM → Montauk 7:41 PM | Crew Book starts at Long Island City before the first public GTFS stop/time; final arrival matches. |

The first four show exactly why the 1-minute matching rule matters. Trains 13, 615 and 621 all have a public departure one minute earlier than the Crew Book departure but line up operationally; the endpoint mismatch is the useful signal.

## Equipment-move findings

- **3304 / Crew 93** is automatically discoverable as Penn Station 4:21 → Port Washington 5:00 from the Crew Book. It is pre-marked **needs timetable review** because the user recalls it is legal E-stop service, but that has not been certified against the current employee timetable.
- **4463 / Crew 166** is automatically discoverable as Great Neck 8:16 PM → GCM 8:53 PM. It is pre-marked **Q-stop / informal** based on the user's direct operating knowledge: usable in real life, but not a legal deadhead and not guaranteed to run at its scheduled time.

The old Revision Builder was useful as a design reference. The included HTML lab deliberately reuses its successful workflow ideas: sticky search/filter controls, one-card-at-a-time review, local autosave, progress count, and JSON export/import. It does **not** modify production EPL.

## Access-rule seed

The Crew Book already supplies some of the access-time data needed by the router. Two particularly strong routing rules are:

- **Penn Station → West Side Yard: 20 minutes** when deadheading to WSY. For a 3:00 PM WSY report, the latest qualifying legal train must arrive Penn by 2:40 PM.
- **Babylon Station → Babylon Yard: 35 minutes** when deadheading to Babylon Yard.

The pack includes additional rules from the release-time pages, but any rule that is written as a release/layup allowance rather than an explicit deadhead allowance is tagged for context review before it can affect routing.

## Safety model for routing

The routing engine should never mix these categories visually or logically:

- **Legal public deadhead** — public scheduled service.
- **Legal employee service** — certified E-stop / employee-only scheduled service.
- **Possible equipment option** — Q-stop/informal movement; never protects report time and should be displayed only as a clearly non-guaranteed alternative.

Only the first two categories may answer “latest legal deadhead.”

## What remains manual

Not train discovery. The manual work becomes **classification/certification**:

- Does this discrepancy really represent an E Stop?
- Is the employee stop pickup, discharge, both, or conditional?
- Is an equipment move a legal employee deadhead or merely a Q-stop practice?
- Are there day/date/timetable restrictions?

That is a much smaller workload than recreating an employee timetable.

## Next technical experiment

The next useful build is a full static-GTFS importer that consumes the matching GO202_26 feed snapshot, normalizes train numbers and stop sequences, and automatically produces the discrepancy queue for every Crew Book passenger movement. The proof cases in this pack show the matching logic is viable.

### Matching rule draft

```text
Candidate public trip = same train number + compatible service day
Origin departure match if:
  GTFS departure == Crew Book departure
  OR GTFS departure == Crew Book departure - 1 minute

Then compare:
  public first stop vs Crew Book origin
  public last stop vs Crew Book destination
  public stop sequence vs Crew Book operational locations

Mismatch => REVIEW QUEUE, never automatic E-stop certification.
```

## Files

- `Deadhead_Equipment_Lab.html` — interactive standalone experiment.
- `equipment_inventory.json` — all extracted indexed equipment movements plus crew mapping and review fields.
- `e_stop_candidate_seed.json` — proof-of-concept public/Crew Book mismatch flags.
- `access_rules_seed.json` — initial access/walking/deadhead rule seed.
- `README_Deadhead_Finder_Research.md` — this report.
