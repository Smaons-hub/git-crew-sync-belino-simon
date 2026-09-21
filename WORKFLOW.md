## Task 1: Push from Clone A
Added overtime pay for shifts over 8 hrs
![Task 1 evidence](screenshots/task1.png)

## Task 2: Conflicting change in Clone B - rejected
Rounded shift pay instead of truncating
![Task 2 evidence](screenshots/task2.png)

## Task 3: Merged the Overtime and Rounding Changes
Merged overtime and rounding changes to resolve conflict. Also updated the test value from 121 to 122.
![Task 3 evidence](screenshots/task3.png)

## Task 4: Diverged Again but is Resolved with Rebase
I intentionally made a conflicting double-time pay change in clone A without fetching first.
the push was rejected. And is resolved via git fetch + git rebase, combining overtime, rounding, and double-time logic into one single function.
![Task 4 conflict](screenshots/task4-firsthalf.png)
![Task 4 resolved push](screenshots/task4-secondhalf.png)