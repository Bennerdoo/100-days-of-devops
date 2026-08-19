# Question

Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:


a. Install nginx on the LBR (load balancer) server if it is not already installed.

b. Configure load-balancing with the http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.

c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all the app servers.

d. Once done, you can access the website by running curl http://stlb01:80 in the terminal.

# Step-by-step Solution

### Step 1: Check Apache Listening Port on App Servers:
Identify Port.
First, check what port Apache is listening on across the app servers:Bash

```Bash
ssh tony@stapp01 "grep -i '^Listen' /etc/httpd/conf/httpd.conf"

```

Output:

```
Listen 3001
```

(Assuming Apache is listening on port 80, 3001, 6200, 6300, or 6400—use that exact port number in the Nginx upstream configuration below).


### Step 2: SSH into Load Balancer Server (stlb01):
Connect to LBR host.Log into stlb01 as user loki:Bash

```Bash
ssh loki@stlb01
```

### Step 3: Install Nginx:
Package installation.
Install Nginx if it is not already installed:Bash

```Bash
sudo yum install -y nginx
```

4. Update /etc/nginx/nginx.conf:
Main config edit.
Backup and overwrite /etc/nginx/nginx.conf with the load balancer configuration. Replace PORT with the actual Apache port found in Step 1 (e.g., 80, 6000, 6300, etc.):Bash

```Bash
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak && \
sudo cat << 'EOF' | sudo tee /etc/nginx/nginx.conf
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    upstream app_servers {
        server stapp01:3001;
        server stapp02:3001;
        server stapp03:3001;
    }

    server {
        listen       80;
        server_name  stlb01;

        location / {
            proxy_pass http://app_servers;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
EOF

```

### Step 5: Validate Syntax and Start Nginx:

Test Nginx syntax, then start and enable the service:Bash

```Bash
sudo nginx -t
sudo systemctl enable --now nginx
sudo systemctl restart nginx
```

### Step 6: Test Load Balancer:

Test the setup from the Jump Host or directly on stlb01:Bash

```Bash
curl http://stlb01:80
```

Expected Result: Returns the web content served from the upstream application servers.