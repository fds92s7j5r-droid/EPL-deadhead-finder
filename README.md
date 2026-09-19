# Deadhead Finder Sandbox v3.0.3

West Side Yard terminal/access hotfix.

## Root cause
v3.0.2 correctly enforced a 20-minute cutoff **when the report location itself was West Side Yard**. But Paper Rev. 8 gives Job 113 a day-specific report-location override of **Penn Station**. The router therefore treated Job 113 as an ordinary Penn report and incorrectly accepted Babylon → Penn arriving only 7 minutes before report.

## Fix
- EPL now keeps two separate concepts:
  - **Crew Book report location** for the day.
  - **Operational/home terminal** for terminal-access legality.
- A West Side Yard assignment remains subject to the **Penn arrival 20 minutes before report** rule even when a revision says to report at Penn Station.
- Job 113 now displays that distinction in the assignment summary.
- Priority comparison and full route detail use the same assignment-aware access rule.
- The v3.0.2 defensive destination-deadline check remains in place.
- Manual GTFS upload remains unchanged.

For the screenshot case: Job 113 reporting 5:01 PM must reach Penn by **4:41 PM**. The Babylon option arriving 4:54 PM must not be labeled legal.
