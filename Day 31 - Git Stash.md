# Question

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/apps present on Storage server in Stratos DC. One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:


Look for the stashed changes under `/usr/src/kodekloudrepos/apps` git repository, and restore the stash with `stash@{1}` identifier. Further, commit and push your changes to the origin.

# Step-by-Step Solution

### Step 1: SSH into Storage Server:

Connect from jump host.Connect to ststor01 from the jump host as user natasha:

```bash
ssh natasha@ststor01
```

### Step 2: Navigate to Repository Directory:

Repository navigation.Change directory to the specified Git repository:

```bash
cd /usr/src/kodekloudrepos/apps
```

### Step 3: List Stashed Changes:

Inspect stashes.List all stashes to locate stash@{1} and verify its contents:

```bash
git stash list
```

### Step 4: Apply stash@{1}:Apply stash.Apply the changes from stash@{1} into your working tree:

```bash
git stash apply stash@{1}
```

### Step 5: Stage and Commit the Restored Changes:

Stage and commit.Stage all applied files and commit them to the current branch:

```bash
git add .
git commit -m "Restored changes from stash@{1}"
```

### Step 6: Push Changes to Remote:

Push changes.Push the new commit to the remote origin:

```bash
git push origin master
```

### Step 7: Verify Repository Status:

Validation.Confirm that the working directory is clean and the commit has been successfully pushed:

```bash
git status
git log --oneline -n 2
```

**Expected Output:** git status shows a clean working tree, and git log shows the new commit at HEAD and origin/master.