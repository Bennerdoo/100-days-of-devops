# Question

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/games` present on `Storage server` in `Stratos DC`. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:

In `/usr/src/kodekloudrepos/games` git repository, revert the latest commit `( HEAD )` to the previous commit (JFYI the previous commit hash should be with `initial commit` message ).

Use `revert games` message (please use all small letters for commit message) for the new revert commit.

# Step-by-Step Solution

### Step 1: SSH into Storage Server:Connect from jump host.
Connect to ststor01 from the jump host as user natasha:
```bash
ssh natasha@ststor01
```

### Step 2: Navigate to Repository Directory:Repository navigation.
Change directory to the specified Git repository:
```bash
cd /usr/src/kodekloudrepos/games
```

### Step 3: Revert HEAD Without Auto-Committing:Revert commit.
Run git revert HEAD with the --no-commit flag to prevent Git from opening the default editor or applying default message templates:
```bash
git revert HEAD --no-commit
```

### Step 4: Commit with Required Message:
Exact message requirement.Commit the revert staging area using the exact required commit message (revert games):
```bash
git commit -m "revert games"
```
then push to origin
```bash
git push origin master
```

### Step 5: Verify Commit History:Validation.
Check the log history to confirm that the new revert commit has been created at HEAD:
```bash
git log --oneline -n 3
```
**Expected Output**:
```
<hash> (HEAD -> master) revert games
<hash> bad/latest commit
<hash> initial commit
```