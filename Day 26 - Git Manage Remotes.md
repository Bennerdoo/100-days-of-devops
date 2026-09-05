# Question

The xFusionCorp development team added updates to the project that is maintained under `/opt/ecommerce.git` repo and cloned under `/usr/src/kodekloudrepos/ecommerce`. Recently some changes were made on Git server that is hosted on Storage server in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on `/usr/src/kodekloudrepos/ecommerce` repository as per details mentioned below:

a. In `/usr/src/kodekloudrepos/ecommerce` repo add a new remote `dev_ecommerce` and point it to `/opt/xfusioncorp_ecommerce.git` repository.

b. There is a file `/tmp/index.html` on same server; copy this file to the repo and add/commit to master branch.

c. Finally push `master` branch to this new remote origin.

# Step-by-Step Solution

### Step 1: SSH into Storage Server:Connect from jump host.
Connect to ststor01 as user natasha:
```bash
ssh natasha@ststor01
```

### Step 2: Navigate to Repository Directory:Repository navigation.
Change directory to the cloned repository location:
```bash
cd /usr/src/kodekloudrepos/ecommerce
```

### Step 3: Add the New Git Remote:

Configure remote.Add the new remote named dev_ecommerce pointing to /opt/xfusioncorp_ecommerce.git:
```bash
git remote add dev_ecommerce /opt/xfusioncorp_ecommerce.git
```

Verify the remote was added correctly:
```bash
git remote -v
```

### Step 4: Copy index.html and Commit to Master:

File operations & commit.Ensure you are on the master branch, copy /tmp/index.html into the working directory, stage, and commit it:

```bash
git checkout master
cp /tmp/index.html .
git add index.html
git commit -m "Add index.html to master branch"
```

### Step 5: Push Master Branch to dev_ecommerce Remote:

Push to new remote.Push the updated master branch to the newly added dev_ecommerce remote:

```bash
git push dev_ecommerce master
```

### Step 6: Verify Commit and Remote Status:

Validation.Confirm that the commit was successfully pushed to the new remote:

```bash
git log --oneline -n 3
```

**Expected Output**: Shows the commit message for index.html with the head pointing to both master and dev_ecommerce/master.