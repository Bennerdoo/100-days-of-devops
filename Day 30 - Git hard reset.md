# Question

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/official` present on `Storage server` in `Stratos DC`. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the `HEAD` and the branch itself to a commit with message `add data.txt file`. Find below more details:


In `/usr/src/kodekloudrepos/official` git repository, reset the git commit history so that there are only two commits in the commit history i.e `initial commit` and `add data.txt file`.

Also make sure to push your changes.

# Step by Step Solution

1. **SSH into Storage Server:**
Connect from jump host.Connect to `ststor01` from the jump host as user `natasha`:
```Bash
ssh natasha@ststor01
```
2. **Navigate to Repository Directory:**
Repository navigation.Change directory to the specified Git repository:
```Bash
cd /usr/src/kodekloudrepos/official
```
3. **Locate the Target Commit Hash:**
Identify commit hash.View the commit log to find the hash corresponding to the message `add data.txt file`:
```Bash
git log --oneline
```
>Note: Take note of the commit hash associated with add data.txt file, e.g., 93ceee8.

4. **Reset HEAD and Working Tree:**
Hard reset operation.Perform a hard reset to point HEAD and the current branch back to that target commit hash:
```Bash
git reset --hard <commit-hash>
```
Replace `<commit-hash>` with the actual commit hash identified in Step 3.

5. **Force Push Changes to Remote:**
Force push to remote.Because the local commit history was rewritten, force push the updated branch to origin to align the remote repository:
```Bash
git push origin master --force
```
(Or `git push origin HEAD --force` depending on the active branch name).

6. **Verify Commit History:**
Validation.Confirm that only the two required commits remain in the commit log:
```Bash
git log --oneline
```
Expected Output: Displays exactly two commits in history:
Plaintext<hash> (HEAD -> master, origin/master) add data.txt file
<hash> initial commit