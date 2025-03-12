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
