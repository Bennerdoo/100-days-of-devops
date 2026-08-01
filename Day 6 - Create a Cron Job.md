# Question

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:


- a. Install cronie package on all Nautilus app servers and start crond service.

- b. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.

# Step-by-step Solution

Execute the following steps on stapp01 (user tony), stapp02 (user steve), and stapp03 (user banner).

### 1. SSH into the App Server:Connect to server.Connect to the target app server:

```Bash
ssh tony@stapp01
```
- (Repeat for steve@stapp02 and banner@stapp03)

### 2. Install cronie package:
Requires sudo.Install the cronie package using yum or dnf:

```Bash
sudo yum install -y cronie
```

### 3. Start and enable crond service:
Enable and start crond.Start the daemon and configure it to run on system boot:
```Bash
sudo systemctl enable --now crond
```
Verify it is active and running:
```Bash
sudo systemctl status crond
```

### 4. Add cron job for root user:
Configure root crontab.Add the specified cron entry to root's crontab using crontab -e or by piping the job directly:
```Bash
(sudo crontab -l 2>/dev/null; echo "*/5 * * * * echo hello > /tmp/cron_text") | sudo crontab -
```

### 5. Confirm root crontab entry:
Verify cron schedule.Check that the cron job was successfully saved for root:
```Bash
sudo crontab -l
```

> Expected output:
```Plaintext
*/5 * * * * echo hello > /tmp/cron_text
```