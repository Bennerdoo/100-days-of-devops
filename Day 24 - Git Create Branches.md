# Question

Nautilus developers are actively working on one of the project repositories, `/usr/src/kodekloudrepos/official`. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:


On Storage server in Stratos DC create a new branch `xfusioncorp_official` from `master` branch in `/usr/src/kodekloudrepos/official` git repo.

Please do not try to make any changes in the code.

# Step by Step Solution

1. **SSH into Storage Server:**

Connect from jump host.
Connect to `ststor01` from the jump host as user `natasha`:
```Bash
ssh natasha@ststor01
```

2. **Navigate to Repository Directory:**

Repository navigation.
Change directory to the specified Git repository:
```Bash
cd /usr/src/kodekloudrepos/official
```

3. **Checkout the Master Branch:**

Ensure source branch.
Ensure you are on the `master` branch before creating the new branch:
```Bash
git checkout master
```

4. **Create the New Branch:**

Branch creation.
Create and switch to the new branch named `xfusioncorp_official`:
```Bash
git checkout -b xfusioncorp_official
```

(Alternatively, create it without switching using `git branch xfusioncorp_official master`)

5. **Verify the Branch List:**

Validation.
Verify that the new branch has been created successfully:
```Bash
git branch
```

**Expected Output:**
```Plaintext
 master
* xfusioncorp_official
```