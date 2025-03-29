# Test-Merge-Request

I'll update what I did step by step, and what's the result.

1. Create a branch from "main" to "develop"
2. Create a branch from "develop" to "develop-feature-A"
3. Make commit A in develop-feature-A branch
4. Make commit A1 in develop-feature-A branch
5. Create PR from "develop-feature-A" to "develop", Squash and Merge
6. Create a branch from "develop" to "main", Squash and Merge.
7. Then let's say I start "develop-feature-B" from "develop"
8. Make commit B in develop-feature-B branch
9. PR from "develop-feature-B" to "develop", Squash and Merge
10. Create a branch from "develop" to "main", Still see the commit A in the File Change.
11. After squash and merge, the commit A File Changes is gone.

---

12. Now I merge from "main" to "develop" first.
13. Then I start "develop-feature-C" from "develop"
14. Make commit C in develop-feature-C branch
15. PR from "develop-feature-C" to "develop", Squash and Merge
16. Create a branch from "develop" to "main", commits diff is there, but File Changes only left C.
17. After squash and merge, the commit C File Changes is gone.

---

18. Now I get the latest from "main" and "develop"
19. Merged from "main" to "develop"
20. Create a branch from "develop" to "develop-feature-D"
21. Make 2 commits D in develop-feature-D branch
22. PR from "develop-feature-D" to "develop", Squash and Merge
23. Now raise PR from "develop" to "main", but instead of Squash and Merge, I choose create a merge request (+1 commit).
24. After create a merge request (+1 commit), When I re-raised PR from "develop" to "main", the Commit & File Changes is empty.

---

25. Let's test on hotfix branch
26. Create a branch from "develop" to "develop-feature-E"
27. Make 2 commits E in develop-feature-E branch
28. Create a branch from "develop" to "develop-feature-F"
29. Make 2 commits F in develop-feature-F branch
30. PR from "develop-feature-E" to "develop", Squash and Merge
31. Let's say "main" need hotfix from feature E.
32. I go to "main" branch, and create a branch from "main" to "main-hotfix-E"
33. Then I cherry picked squashed commit E from "develop-feature-E" to "main-hotfix-E", by hash
34. Now I raise PR from "main-hotfix-E" to "main", create a merge request (+1 commit).
35. Then I merge "develop-feature-F" to "develop", Squash and Merge
36. Now I raise PR from "develop" to "main"
37. I see both commit E and commit F, and File Changes in E and F
38. After normal merge, got +1 commit in "main", and commit in F only (E is gone due to matching commit hash)
39. When I re-raised PR from "develop" to "main", the Commit & File Changes is empty.

---

40. Let's test "rebase and merge"
41. Create a branch from "develop" to "develop-feature-G"
42. Make 2 commits G in develop-feature-G branch
43. Create a branch from "develop" to "develop-feature-H"
44. Make 2 commits H in develop-feature-H branch

45. PR from "develop-feature-G" to "develop", Squash and Merge
46. PR from "develop-feature-H" to "develop", Squash and Merge

47. Now I raise PR from "develop" to "main", use rebase and merge
48. I see all the latest commits in "develop" are on top of "main"
49. When I try to merge from "develop" to "main", it conflicted.
50. Then I have to merge from "main" to "develop", resolve the conflicts, run git merge --continue, then re-raise the PR
51. Then I can reate a merge request (+1 commit) and merge

Hence conclusion is:

1. From "feature-branch" to "develop", it's better to use Squash and Merge (cleaning Dev) OR Rebase and Merge (Have clear steps in Git History).
2. From "develop" to "main", it's better to use create a merge request (+1 commit).
3. From "hotfix-branch" to "main", it's better to use create a merge request (+1 commit).

Note:

1. Restriction of feature to dev > only squash and merge (can set via branch rule)
2. Restriction of dev to main > only create a merge request (+1 commit) || normal merge is allowed. (can set via branch rule)
3. Then everything will be clean.

## 29/03/2025 - Test on hotfix and see merge behavior

1. Main have +1 commit compared to develop, and they are synced.
2. I created update on commit B, C, D to Develop
3. Note that I'm going to hotfix C into Main. C in Develop now have hash 457dd772d0adbd0f5a813942bdb70846668aba23.
4. To follow production practice, we will always have to member to review first, before merging to Main.
5. Hence, I'll create a hotfix branch from main, cherry pick from develop via hash, and raise PR to main.
   a. Created hotfix/c-feature, cherry picked from Develop, raised PR.
   b. Now note that the exact name commit C, have different hash in prod bea3b32cae8943efb01feb2ad0ea5289d2b5850f.
6. Okay, now let's try to deploy from develop to main.
   a. No conflict, but feature C is displaying the changes.
7. Now, Let's try merge from main to develop, to sync both branch
   a. After this change, feature C is not longer been displayed on the PR from develop to main.
8. Now, Let's say feature C got bug, and another hotfix is required, while there's new commit E to Develop.
9. Step 5 is repeated, and now main have +1 hotfix commit.
10. Say this time, instead of syncing with dev, we continue code in Develop, and realize that feature C need another hotfix.
11. And then hotfix to deploy to main again, and create PR and merge again.
12. So now everything is fine.
13. What if the next hotfix of C, plan to be deploy together with the rest? Let's try
14. And there will be a conflict!
    a. This happened because, main and dev have different code; so git don't know which to accept.
15. Let's try create a branch from develop, and merge with main
    a. Resolve conflict in the file
    b. Run git status, and follow the step: git add xxx
    c. git commit with message
16. let's merge from this resolve branch to dev.
17. Then in the PR of dev to production, the conflict is resolved!
18. It will take ALL the commits and make them sync!

Note:

1. "Merge" means comparing both branch, putting in the commits that are missing, into the other branch.
2. The +1 commit is important, to jot down any resolved conflict information, so that it won't be conflicting the next time.
