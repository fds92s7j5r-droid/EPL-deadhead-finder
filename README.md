# Engineer Pay Log — Deadhead Finder Sandbox v3.4.0

## Home Origin Resolver + LIC/Hunterspoint

The architectural change in this build is that **report location and home origin are now separate**.

### First proof case: Job 7, weekday
- Reports: Jamaica Storage Yard
- To work: Jamaica 20 minutes before report
- Final working train: Train 13
- Train 13 arrives Long Island City: 2:56 PM
- Going home: Long Island City, not JSY
- Five minutes from physical availability is paper legal at LIC
- S/E stops are guaranteed
- Q stops are extremely high-confidence
- Under-five-minute LIC connections may be shown as practical but never protected

### Curated LIC / HPT origins
62 E 8:37 AM
512 Q 8:42 AM
5214 E 9:54 AM → Jamaica E 10:15 AM
8 E 11:18 AM
10 E 11:18 AM
656 S 3:15 PM → HPT 3:30 PM
558 Q 3:42 PM → HPT 3:57 PM
658 Q 4:07 PM → HPT 4:22 PM
18 Q 4:16 PM → HPT 4:30 PM
80 S 4:27 PM → HPT 4:42 PM
662 S 4:58 PM → HPT 5:07 PM
698 S 5:43 PM → HPT 5:58 PM
568 S 6:40 PM → HPT 6:53 PM

### Train 8 / Train 10 special calendar
Train 8 runs Monday–Friday except:
- Fridays 2026-05-28 through 2027-09-03
- Thursday 2027-06-17
- Thursday 2027-07-01

Train 10 runs on those exception Fridays and those two Thursdays.

### Tiny test
Try **Job 7 — Monday Sep. 21 — Ronkonkoma**.

Expected:
1. To-work side still uses JSY / Jamaica −20.
2. Going-home header says **Long Island City** and references Train 13 at **2:56 PM**.
3. JSY +20 and Bolands disappear from the home side.
4. Curated LIC-origin trains can participate in the route.
