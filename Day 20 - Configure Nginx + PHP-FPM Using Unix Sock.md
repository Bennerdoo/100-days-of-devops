# Question

The `Nautilus` application development team is planning to launch a new PHP-based application, which they want to deploy on `Nautilus` infra in `Stratos DC`. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:


- a. Install `nginx` on `app server 3`, configure it to use port `8093` and its document root should be `/var/www/html`.

- b. Install `php-fpm` version `8.3` on `app server 3`, it must use the unix socket `/var/run/php-fpm/default.sock` (create the parent directories if don't exist).

- c. Configure `php-fpm` and `nginx` to work together.

- d. Once configured correctly, you can test the website using `curl http://stapp03:8093/index.php` command from jump host.

NOTE: We have copied two files, `index.php` and `info.php`, under `/var/www/html` as part of the `PHP-based application` setup. Please do not modify these files.

# Step-by-Step Solution

Execute these commands on stapp03:

### Step 1. SSH into App Server 3:
Connect from jump host.Log into stapp03 as user banner:
```Bash
ssh banner@stapp03
```

### Step 2. Install Nginx, PHP 8.3, and PHP-FPM:
Package installation.
Enable the Remi repository (or module streams) to install PHP 8.3 along with Nginx and PHP-FPM:
```Bash
# Reset existing php module streams and enable php:8.3 / remi-8.3
sudo dnf module reset php -y
sudo dnf module enable php:remi-8.3 -y 2>/dev/null || sudo dnf module enable php:8.3 -y 2>/dev/null || true

# Install packages
sudo dnf install -y nginx php-fpm php-cli
```

### Step 3. Configure PHP-FPM Unix Socket:
Socket configuration.
Create the parent directory for the UNIX socket if it does not exist:

```Bash
sudo mkdir -p /var/run/php-fpm
```

Edit the PHP-FPM pool configuration (/etc/php-fpm.d/www.conf) to use the target socket and set permissions for Nginx:

```Bash
sudo sed -i 's|^listen = .*|listen = /var/run/php-fpm/default.sock|' /etc/php-fpm.d/www.conf
sudo sed -i 's/^;listen.owner = .*/listen.owner = nginx/' /etc/php-fpm.d/www.conf
sudo sed -i 's/^;listen.group = .*/listen.group = nginx/' /etc/php-fpm.d/www.conf
sudo sed -i 's/^;listen.mode = .*/listen.mode = 0660/' /etc/php-fpm.d/www.conf
sudo sed -i 's/^user = .*/user = nginx/' /etc/php-fpm.d/www.conf
sudo sed -i 's/^group = .*/group = nginx/' /etc/php-fpm.d/www.conf
4.4. Configure Nginx on Port 8093 with PHP-FPM:Nginx web server setup.Update /etc/nginx/nginx.conf to listen on port 8093, set /var/www/html as document root, and pass .php requests to the UNIX socket:Bashsudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak && \
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

    server {
        listen       8093;
        listen       [::]:8093;
        server_name  stapp03;
        root         /var/www/html;
        index        index.php index.html;

        location / {
            try_files $uri $uri/ =404;
        }

        location ~ \.php$ {
            fastcgi_pass   unix:/var/run/php-fpm/default.sock;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
    }
}
EOF
```

### Step 4. Adjust Document Root Permissions:
Directory permissions.
Ensure Nginx has ownership and read permissions over /var/www/html without editing index.php or info.php:
```Bash
sudo chown -R nginx:nginx /var/www/html
sudo chmod -R 755 /var/www/html
```

### Step 5. Start & Enable PHP-FPM and Nginx Services:
Service startup.
Verify configuration syntax and start both services:
```Bash
sudo nginx -t
sudo systemctl enable --now php-fpm
sudo systemctl enable --now nginx
sudo systemctl restart php-fpm nginx
```

### Step 6. Verify Access from Jump Host:
Verification.
Exit back to the Jump Host and test reachability:
```Bash
curl http://stapp03:8093/index.php
curl http://stapp03:8093/info.php
```
**Expected Output:** Successfully renders output from the pre-existing PHP files.