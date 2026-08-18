# Question

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 2 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:


1. Install and configure nginx on App Server 2.

2. On App Server 2 there is a self signed SSL certificate and key present at location `/tmp/nautilus.crt` and `/tmp/nautilus.key`. Move them to some appropriate location and deploy the same in Nginx.

3. Create an `index.html` file with content `Welcome!` under Nginx document root.

4. For final testing try to access the App Server 2 link (via hostname) from jump host using curl command. For example: `curl -Ik https://<app-server-name>/`.

# Step-by-Step Solution

### Step 1: SSH into App Server 2:

Connect from jump host.Log into stapp02 as user steve:Bash

```Bash
ssh steve@stapp02
```

### Step 2: Install Nginx:

Requires sudo.Install the Nginx web server package using yum:Bash

```Bash
sudo yum install -y nginx
```

### Step 3: Move SSL Certificate and Key:

Certificate setup.Create standard SSL storage directories and move the certificate and key from /tmp/:Bash
```Bash
sudo mkdir -p /etc/pki/nginx/private /etc/pki/nginx/certs
sudo mv /tmp/nautilus.crt /etc/pki/nginx/certs/nautilus.crt
sudo mv /tmp/nautilus.key /etc/pki/nginx/private/nautilus.key

# Set appropriate secure file permissions
sudo chmod 600 /etc/pki/nginx/private/nautilus.key
sudo chmod 644 /etc/pki/nginx/certs/nautilus.crt
```

### Step 4: Create index.html File:

Document Root Content.Ensure Nginx's default document root directory exists and create the required index.html file:Bash

```Bash
sudo mkdir -p /usr/share/nginx/html
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```

### Step 5: Configure Nginx for HTTPS:

Configure SSL in Nginx.Edit the default Nginx configuration file:Bash

```Bash
sudo vi /etc/nginx/nginx.conf
```

Ensure the server block is configured (or uncomment the HTTPS server block) to serve traffic over SSL on port 443:Nginx
```Nginx
server {
    listen       443 ssl http2;
    listen       [::]:443 ssl http2;
    server_name  stapp02;
    root         /usr/share/nginx/html;

    ssl_certificate "/etc/pki/nginx/certs/nautilus.crt";
    ssl_certificate_key "/etc/pki/nginx/private/nautilus.key";

    location / {
        index index.html;
    }
}
```

### Step 6: Test Syntax & Start Nginx:

Start & Enable service.Verify the configuration syntax, then enable and start the service:Bash
```Bash
sudo nginx -t
sudo systemctl enable --now nginx
```

### Step 7: Verify HTTPS Accessibility from Jump Host:

Curl test from Jump Host.Exit back to the Jump Host and test the deployment using curl:

```Bash
curl -Ik https://stapp02/
```

Expected Output Snippet:

```text
HTTP/1.1 200 OK
Server: nginx/...
Content-Type: text/html
```