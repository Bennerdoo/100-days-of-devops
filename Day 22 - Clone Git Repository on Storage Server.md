# Question

The DevOps team established a new Git repository last week, which remains unused at present. However, the Nautilus application development team now requires a copy of this repository on the Storage Server in the Stratos DC. Follow the provided details to clone the repository:


The repository to be cloned is located at `/opt/media.git`

Clone this Git repository to the `/usr/src/kodekloudrepos` directory. Perform this task using the `natasha` user, and ensure that no modifications are made to the repository or existing directories, such as changing permissions or making unauthorized alterations.

# Step-by-Step Solution

### Step 1: SSH into Storage Server:
Connect from jump host.Connect to ststor01 from the jump host as user natasha:
```Bash
ssh natasha@ststor01
```
### Step 2: Verify Target Directory:
Ensure directory exists.Ensure the target destination directory /usr/src/kodekloudrepos exists (create it if missing, without altering existing permissions):
```Bash
sudo mkdir -p /usr/src/kodekloudrepos
```
### Step 3: Clone the Bare Repository as User natasha:
Clone repository.Clone /opt/media.git directly into /usr/src/kodekloudrepos/media:
```Bash
cd /usr/src/kodekloudrepos
git clone /opt/media.git
```
(If sudo access is needed to write inside `/usr/src/kodekloudrepos`, ensure the cloned files retain natasha ownership):
```Bash
sudo git clone /opt/media.git /usr/src/kodekloudrepos/media
sudo chown -R natasha:natasha /usr/src/kodekloudrepos/media
```
### Step 4: Verify Cloned Repository Status:
Validation.Check that the repository was successfully cloned:
```Bash
cd /usr/src/kodekloudrepos/media
git status
```
**Expected Output:** Displays standard git status (e.g., On branch main or On branch master) without any errors.