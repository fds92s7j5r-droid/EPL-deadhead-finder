# Deadhead Finder Sandbox v3.0.2

Hotfix after West Side Yard legality testing.

## Fixed
- A to-work route is now defensively rejected unless its **actual destination arrival** is on or before the calculated access deadline.
- West Side Yard: Penn Station arrival deadline = **report time minus 20 minutes**.
- Jamaica Storage Yard uses the same strict deadline concept with its configured 20-minute Jamaica access allowance.
- The priority-location comparison now shows:
  - actual station arrival
  - minutes before report
  - the special access deadline (for example, Penn 4:41 PM for a 5:01 PM WSY report)
- A route that misses the special access deadline can no longer be labeled a legal to-work route.
- Manual GTFS upload behavior from v3.0.1 is unchanged.

This remains a sandbox and does not modify Engineer Pay Log production.
