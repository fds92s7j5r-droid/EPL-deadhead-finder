# Deadhead Finder Sandbox v3.0.4

Early-AM / previous-night routing hotfix.

## Root cause
The to-work graph only loaded GTFS service for the assignment's report date. That works for most jobs, but not for jobs reporting shortly after midnight. A Job 1 or Job 2 report around 12:35–12:40 AM may require a deadhead that departed on the previous calendar day.

## Fix
- To-work routing now loads **previous day + work date** service together.
- GTFS trips represented as 24:xx / 25:xx on the prior service day are shifted correctly into the work-date midnight hours.
- The normal 8-hour latest-route search can now reach back into the previous evening.
- Previous-night departures display the correct weekday label.
- Route-specific Community Notes use the actual service day of the deadhead train.
- Early-AM jobs show a testing message confirming that previous-calendar-day service is being searched.
- West Side Yard / Jamaica Storage Yard access cutoffs from v3.0.3 remain unchanged.
- Manual GTFS upload remains unchanged.

## Good tests
- Job 1: 12:35 AM Jamaica Storage Yard report. Legal deadhead must reach Jamaica by 12:15 AM, and the route may depart on the prior calendar day.
- Job 2: 12:40 AM Jamaica Storage Yard report. Legal deadhead must reach Jamaica by 12:20 AM.
- Compare a Monday early-AM job against Sunday-night service to make sure weekend/weekday crossover is handled correctly.
