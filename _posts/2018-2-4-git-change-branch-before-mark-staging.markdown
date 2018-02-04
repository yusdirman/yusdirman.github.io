---
layout: post
title:  "Checkout to other Branch or new before staging to bring over all edited lines to the new created branch"
date:   2018-02-04 09:05:00 +0800
categories: Git
---

# Checkout to other branch before staging to bring over all edited lines to the destination branch


Assalamualaikum...

Often in my environment situation especially during debugging process, I start editing the codes at any branch I was on to investigate or to test proof my solution for repair, before I create a branch for the fix.

When the solution was proven and the solution is acceptable to implement and test, only then I create a branch in our gitlab interface and then create a Merge Request(MR) right away.

```bash

# in local

me@local:$\> git fetch
From gitlab.mydomain.com:project/application
 * [new branch]      520-[BUG]-issue-name-and-simple-descriptions -> origin/520-[BUG]-issue-name-and-simple-descriptions

# when I check my current status

me@local$\> git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   app/controllers/departments_controller.rb
	modified:   app/views/departments/show.html.haml

no changes added to commit (use "git add" and/or "git commit -a")


# just checkout to the new created origin branch
$\> git co -b  520-[BUG]-issue-name-and-simple-descriptions  origin/520-[BUG]-issue-name-and-simple-descriptions

# check status
me@local$\> git status
On branch 520-[BUG]-issue-name-and-simple-descriptions
Your branch is up-to-date with 'origin/520-[BUG]-issue-name-and-simple-descriptions '.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   app/controllers/departments_controller.rb
	modified:   app/views/departments/show.html.haml

no changes added to commit (use "git add" and/or "git commit -a")
```

And now, the changes is in the new created branch from MR the issue in gitlab.

### Cautions
- please use a new branch or a clean destination branch. Otherwise it would return error and you cannot checkout to the branch.
- Must be in UnStage state. Edit the code and save. Don't add tu staging state and of course don't commit for this to work.


## What if I've already commited and then decided that all the modified code should be in a new issue/branch?

just run reset soft to return to staged state. Then reset all files to return to unstaged state. And then, change to new branch and stage and commit from there.







Thanks for reading.
Assalamualaikum



yusdirman
