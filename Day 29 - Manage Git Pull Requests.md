# Question

Max want to push some new changes to one of the repositories but we don't want people to push directly to master branch, since that would be the final version of the code. It should always only have content that has been reviewed and approved. We cannot just allow everyone to directly push to the master branch. So, let's do it the right way as discussed below:

SSH into storage server using user max, password Max_pass123 . There you can find an already cloned repo under Max user's home.

Max has written his story about The 🦊 Fox and Grapes 🍇

Max has already pushed his story to remote git repository hosted on Gitea branch story/fox-and-grapes

Check the contents of the cloned repository. Confirm that you can see Sarah's story and history of commits by running git log and validate author info, commit message etc.

Max has pushed his story, but his story is still not in the master branch. Let's create a Pull Request(PR) to merge Max's story/fox-and-grapes branch into the master branch

Click on the Gitea UI button on the top bar. You should be able to access the Gitea page.

UI login info:
- Username: max
- Password: Max_pass123
PR title : Added fox-and-grapes story
PR pull from branch: story/fox-and-grapes (source)
PR merge into branch: master (destination)

Before we can add our story to the master branch, it has to be reviewed. So, let's ask tom to review our PR by assigning him as a reviewer


Add tom as reviewer through the Git Portal UI
Go to the newly created PR

Click on Reviewers on the right

Add tom as a reviewer to the PR
Now let's review and approve the PR as user Tom


Login to the portal with the user tom

Logout of Git Portal UI if logged in as max

UI login info:
- Username: tom
- Password: Tom_pass123
PR title : Added fox-and-grapes story
Review and merge it.
Great stuff!! The story has been merged! 👏

Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

# Step-by-Step Solution

### Step 1: SSH into Storage Server & Inspect Repository:
**Terminal Verification.**
SSH into ststor01 as user max:
```Bash
ssh max@ststor01
```
**Password:** Max_pass123

**Navigate to the repository in Max's home directory and verify the commit history:**
```Bash
cd ~/story-blog   # Or the respective repository directory in ~/
git log --oneline -n 5
```

### Step 2: Create Pull Request in Gitea UI:
Logged in as max.

Click the Gitea UI button on the top navigation bar.

Sign in with:

Username: max
Password: Max_pass123
Navigate to the repository (story-blog).

Click on Pull Requests tab > New Pull Request.

Set the branch merge targets:

Target/Destination branch: master

Source/Compare branch: story/fox-and-grapes

Click New Pull Request.

Set the title to: Added fox-and-grapes story

Click Create Pull Request.

### 3. Assign tom as Reviewer:Assign Reviewer.
On the newly created PR page, locate the Reviewers section in the right sidebar.Click on Reviewers and search/select tom.Confirm tom is added as a reviewer to the PR.

### 4. Review & Merge Pull Request as tom:
Logged in as tom.
Log out of Gitea as max.
Log back into Gitea with:Username: tomPassword: Tom_pass123Open the repository and go to Pull Requests.Click on the PR titled Added fox-and-grapes story.Click Files Changed to review the changes.Click Review changes -> Approve -> Submit review.Click the green Merge Pull Request button and confirm the merge.

### 5. Confirm Merge Completion:
**Validation.**
Ensure the PR status changes to Merged. Take screenshots of the merged PR status if required for review.