# Question
The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for archiving website content files. They have a static website running on App Server 1 in Stratos Datacenter, and they need to create a bash script named news_archive.sh which should accomplish the following tasks. (Also remember to place the script under the /scripts directory on App Server 1).


- **a.** Create a zip archive named xfusioncorp_news.zip of /var/www/html/news directory.

- **b.** Save the archive in the /archives/ directory on the App Server 1. This is a temporary storage, as archives from this location will be cleaned on a weekly basis. Therefore, the archive should also be copied to the Nautilus Storage Server so it can be retrieved later for validation purposes.

- **c.** Copy the created archive to the Nautilus Storage Server server in the /archives/ location.

- **d.** Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.

- **e.** Do not use sudo inside the script.

- **Note:**
The zip package must be installed on given App Server before executing the script. This package is essential for creating the zip archive of the website files. Install it manually outside the script.

# Step-by-Step Solution

1. SSH into App Server 1:Connect to App Server 1.SSH into App Server 1 from the jump host as user tony:
```Bash
ssh tony@stapp01
```

2. Install zip package:Prerequisite package.Install zip on App Server 1 manually as required:Bashsudo yum install -y zip
```Bash
sudo yum install -y zip
```

3. Configure SSH Key for tony to Storage Server:Setup Passwordless SCP.Generate an SSH key pair for user tony (press Enter for all prompts) and copy the public key to ststor01 (user natasha):Bashssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
ssh-copy-id natasha@ststor01
```Bash
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
ssh-copy-id natasha@ststor01
```

Test passwordless SSH to verify:
```Bash
ssh natasha@ststor01 "hostname"
```

4. Create necessary local directories:Directory setup.Ensure /scripts and /archives exist locally on stapp01 with correct permissions for tony:
```Bash
sudo mkdir -p /scripts /archives
sudo chown -R tony:tony /scripts /archives
```

5. Write /scripts/news_archive.sh:Create bash script.Create the script file:
```Bash
vi /scripts/news_archive.sh
```

Add the following content:
```Bash

# Define variables
SRC_DIR="/var/www/html/news"
ARCHIVE_DIR="/archives"
ARCHIVE_NAME="xfusioncorp_news.zip"
REMOTE_USER="natasha"
REMOTE_HOST="ststor01"
REMOTE_DIR="/archives/"

# Create zip archive in /archives/
zip -r "${ARCHIVE_DIR}/${ARCHIVE_NAME}" "${SRC_DIR}"

# Copy archive to Nautilus Storage Server without password prompt
scp "${ARCHIVE_DIR}/${ARCHIVE_NAME}" "${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}"
```

Make the script executable:
```Bash
chmod +x /scripts/news_archive.sh
```

6. Test Script Execution:Run and verify.Execute the script as user tony (without sudo):
```Bash
/scripts/news_archive.sh
```

Verify the archive exists locally and on the storage server:
```Bash
ls -l /archives/xfusioncorp_news.zip
ssh natasha@ststor01 "ls -l /archives/xfusioncorp_news.zip"