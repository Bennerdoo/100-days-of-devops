# Question

The Nautilus application development team has been working on a project repository `/opt/cluster.git`. This repo is cloned at `/usr/src/kodekloudrepos` on storage server in Stratos DC. They recently shared the following requirements with the DevOps team:


There are two branches in this repository, `master` and `feature`. One of the developers is working on the `feature` branch and their work is still in progress, however they want to merge one of the commits from the `feature` branch to the `master` branch, the message for the commit that needs to be merged into `master` is `Update info.txt`. Accomplish this task for them, also remember to push your changes eventually.

# Step-by-Step Solution

### Step 1: SSH into Storage Server:
Connect from jump host:
```bash
ssh natasha@ststor01
```

### Step 2: Navigate to the Repository:
Change to the directory where the repository is cloned:
```bash
cd /usr/src/kodekloudrepos/cluster
```

### Step 3: List Available Branches:
Verify both `master` and `feature` branches exist:
```bash
git branch -a
```

### Step 4: Get the Commit Hash to Cherry-Pick:
Identify the specific commit you need to merge (the one with the message "Update info.txt" on the feature branch):
```bash
git log --oneline --all
```

Note down the full SHA-1 hash of the commit message "Update info.txt". Let's assume it is `a1b2c3d4` (replace with the actual hash from your log).

### Step 5: Switch to the Master Branch:
Checkout the target branch where you want to apply the commit:
```bash
git checkout master
```

### Step 6: Cherry-Pick the Commit:
Apply the specific commit from the feature branch to the master branch using the commit hash you noted earlier:
```bash
git cherry-pick a1b2c3d4
```
*(Replace `a1b2c3d4` with the actual hash)*

### Step 7: Push Changes to Origin:
After successful cherry-picking, push the updated master branch to the remote repository:
```bash
git push origin master
```

### Step 8: Verification (Optional):
Check the logs on both branches to confirm the change was applied correctly:
```bash
git log --oneline master
git log --oneline feature
```
The commit should now appear in the history of both branches.
