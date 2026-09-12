# Question

The Nautilus application development team has been working on a project repository /opt/apps.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:


One of the developers is working on feature branch and their work is still in progress, however there are some changes which have been pushed into the master branch, the developer now wants to rebase the feature branch with the master branch without loosing any data from the feature branch, also they don't want to add any merge commit by simply merging the master branch into the feature branch. Accomplish this task as per requirements mentioned.

Also remember to push your changes once done.

# Step-by-Step Solution

### Step 1: SSH into Storage Server:

Connect from the jump host to `ststor01` as user `natasha`:

```bash
ssh natasha@ststor01
```

### Step 2: Navigate to Repository Directory:

Change directory to the repository located at `/usr/src/kodekloudrepos`:

```bash
cd /usr/src/kodekloudrepos
```

### Step 3: Switch to the Feature Branch:

Move to the `feature` branch where the in-progress work is located:

```bash
git checkout feature
```

### Step 4: Rebase the Feature Branch:

Rebase the `feature` branch onto the `master` branch. This rewrites the `feature` branch's history to appear as if it was branched from the latest `master` commit, without creating a merge commit.

```bash
git rebase master
```

### Step 5: Push the Changes to Remote:

Since rebase rewrites history, a force push is required to update the remote `feature` branch.

```bash
git push origin feature --force
```

### Step 6: Verification:

Verify that the rebase was successful and the history is linear:

```bash
git log --oneline
```

**Expected Output:** You should see the commits from the `feature` branch now appearing *after* the latest commit from the `master` branch, forming a single linear history.