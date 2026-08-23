# Question

xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:


- a. Install `httpd` package and dependencies on `app server 1`.

- b. Apache should serve on port `8088`.

- c. There are two website's backups `/home/thor/blog` and `/home/thor/cluster` on `jump_host`. Set them up on Apache in a way that `blog` should work on the link `http://localhost:8088/blog/` and `cluster` should work on link `http://localhost:8088/cluster/` on the mentioned app server.

- d. Once configured you should be able to access the website using curl command on the respective app server, i.e `curl http://localhost:8088/blog/` and `curl http://localhost:8088/cluster/`


# Step-by-step Solution

### Step 1. Copy Website Backups to App Server 1:
From Jump Host.
From the Jump Host, copy the backup directories from /home/thor/ directly to stapp01's Apache document root (/var/www/html/):
```Bash
scp -r /home/thor/blog tony@stapp01:/tmp/
scp -r /home/thor/cluster tony@stapp01:/tmp/
```

### Step 2. SSH into App Server 1:
Connect from jump host.
Log into stapp01 as user tony:
```Bash
ssh tony@stapp01
```

### Step 3. Install httpd and Move Website Files:Package setup.
Install Apache if not already present, move the website backup folders into /var/www/html/, and set permissions:Bash
```Bash
sudo yum install -y httpd

# Move folders into document root
sudo mv /tmp/blog /var/www/html/
sudo mv /tmp/cluster /var/www/html/

# Set proper permissions
sudo chmod -R 755 /var/www/html/blog /var/www/html/cluster
```

### Step 4. Configure Apache to Listen on Port 8088:Port configuration.
Update Apache's Listen directive in /etc/httpd/conf/httpd.conf:
```Bash
sudo sed -i 's/^Listen .*/Listen 8088/' /etc/httpd/conf/httpd.conf
```
Check for duplicate Listen lines in secondary config files to prevent startup conflicts:
```Bash
grep -rn "Listen" /etc/httpd/
```

### Step 5. Kill Any Conflicting Process on Port 8088:Free port if occupied.Check if port 8088 is occupied by another process and clear it:Bash
```Bash
sudo fuser -k 8088/tcp 2>/dev/null || true

### Step 6. Test Syntax & Start httpd Service:Service management.
Test configuration syntax, start, and enable the Apache service:
```Bash
sudo httpd -t
sudo systemctl enable --now httpd
sudo systemctl restart httpd
```
### Step 7. Verify URLs on App Server 1:Validation.Run the curl commands directly on stapp01 to test access:
```Bash
curl -I http://localhost:8088/blog/
curl -I http://localhost:8088/cluster/
```
Expected Result: Both commands return HTTP/1.1 200 OK.