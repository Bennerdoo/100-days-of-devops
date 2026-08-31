# Question

The Nautilus application development team has been working on a project repository `/opt/apps.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`. They recently shared the following requirements with DevOps team:


- Create a new branch `devops` in `/usr/src/kodekloudrepos/apps` repo from `master` and copy the `/tmp/index.html` file (present on `storage server` itself) into the repo. Further, `add/commit` this file in the new branch and merge back that branch into `master` branch. Finally, push the changes to the origin for both of the branches.

# Step-by-Step Solution

### Step 1: SSH into Storage Server

```bash
ssh peter@ststor01
```

### Step 2: Navigate to the Repository Directory

```bash
sudo su
cd /usr/src/kodekloudrepos/apps
```

### Step 3: Create and Switch to the New Branch

```bash
git checkout -b devops
```

### Step 4: Copy the File and Add it to Git

```bash
sudo cp /tmp/index.html .
git add index.html
git commit -m "Add index.html to devops branch"
```

### Step 5: Merge the Branch Back into Master

```bash
git checkout master
git merge devops
```

### Step 6: Push Both Branches to Origin

```bash
git push origin devops
git push origin master
```