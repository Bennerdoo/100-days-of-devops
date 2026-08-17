# Question

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.


Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don't need to worry if Apache isn't serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 6400 on all app servers.

# Step-by-Step Solution

***Execute these steps across stapp01, stapp02, and stapp03:***

### Step 1: SSH into the App Server

Connect from jump host.Connect to the target application server from the jump host:

```Bash
ssh tony@stapp01   # For stapp01
# ssh steve@stapp02  (for stapp02)
# ssh banner@stapp03 (for stapp03)
```

### Step 2: Set Apache Port to 6400

Fix configuration.Check and update Apache's primary configuration file to listen on port 6400:

```Bash
sudo sed -i 's/^Listen .*/Listen 6400/' /etc/httpd/conf/httpd.conf
```

**Check for duplicate Listen directives in secondary config files to prevent Address already in use startup errors:**

```Bash
grep -rn "Listen" /etc/httpd/
```

If duplicate Listen 6400 or Listen 80 entries exist in /etc/httpd/conf.d/*.conf, remove or comment them out.

### Step 3: Terminate Any Conflicting Process on Port 6400

**Clear port conflicts.**Check if an orphaned process is holding port 6400:

```Bash
sudo lsof -i :6400
```

If a process is occupying the port, kill it:

```Bash
sudo fuser -k 6400/tcp
# Or using lsof:
# sudo kill -9 $(sudo lsof -t -i:6400)
```

### Step 4: Test Syntax & Enable httpd Service

**Start service.**Test configuration syntax and start the service:

```Bash
sudo httpd -t
sudo systemctl enable --now httpd
sudo systemctl restart httpd
sudo systemctl status httpd --no-pager
```

### Step 5: Verify Apache Status Across All Servers

**Verification.**Exit to the Jump Host and confirm Apache is active and listening on port 6400 across all three app servers:

```Bash
curl -I http://stapp01:6400
curl -I http://stapp02:6400
curl -I http://stapp03:6400
```

**Expected Result:** Each server responds with HTTP status headers (or content) without connection timeouts or connection refused errors.