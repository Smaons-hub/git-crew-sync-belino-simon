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

## Task 5: Merge back into Main
I merged the finished feature/overtime-pay branch into main and pushed the update to
GitHub.
![Task 5 evidence](screenshots/task5.png)

## Task 6: Tag and Finalize
Tagged the final synced commit as v1.0-synced and pushed the tag to GitHub
![Task 6 evidence](screenshots/task6.png)

## WORKFLOW.md Questions

**1. What did the rejected push error message tell you, and why did it happen?**

When I tried to push from Clone B in Task 2, I got:
! [rejected] feature/overtime-pay -> feature/overtime-pay (non-fast-forward)

This happened because Clone B's local feature/overtime-pay branch was still
pointing at an older commit — it had no idea Clone A had already pushed the
overtime pay change. Git refuses to push when your local branch and the
remote branch have diverged, because a plain push can only move the remote
pointer forward (a fast-forward). If it let the push through anyway, Clone
A's commit would effectively be overwritten and lost. Git forces you to pull
in the missing history first so you can reconcile both sets of changes
deliberately, instead of silently discarding someone else's work.

**2. What's the actual difference between how you resolved Task 3 (merge) vs Task 4 (rebase)?**

In Task 3, I ran git fetch + git merge, which created a new merge commit
(c69ff7e) with two parents — my rounding commit and Clone A's overtime
commit. Both original commit histories stayed intact and visible side by
side.

In Task 4, I used git fetch + git rebase instead, which replayed my
double-time commit on top of the already-merged history rather than joining
two branches. The result was a single, linear line of commits with no merge
commit at all.

The rebase also taught me something the merge didn't: my first rebase
attempt actually succeeded with no conflict at all, but when I checked the
resulting code, git had silently kept the other branch's version and
quietly dropped my entire double-time change, because our edits happened
not to touch the exact same lines. Tests still passed, so nothing looked
wrong — I only caught it by manually re-reading the merged function. That
was the biggest lesson of the lab: a clean, conflict-free merge or rebase is
not proof the result is correct.

**3. What one habit would have avoided both rejected pushes in this lab?**

Running git fetch (or git pull) right before starting new work, and again
right before pushing. Both rejections happened because I made changes based
on a local branch that was already out of date.

**4. Which approach — merge or rebase — would you default to on a shared team branch, and why?**

I'd default to merge for a branch that's already shared/pushed, and save
rebase for cleaning up my own local, unpushed commits before I share them.
Merge preserves the real history of what actually happened, which matters
once a branch has other people's commits on it — rewriting published
history with rebase risks breaking teammates whose work already depends on
those commits. Rebase is great for tidying my own commits before anyone
else has seen them, but Task 4 also showed me it can mask real conflicts by
auto-combining changes without alerting me, which makes it riskier to trust
blindly on a shared branch.