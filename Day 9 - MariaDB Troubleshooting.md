# Question

There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server.


Look into the issue and fix the same.

# Step-by-Step Solution

## 1. SSH into stdb01 & Switch to Root:

From Jump Host.Connect to the database server and escalate to root privileges:

```Bash
ssh peter@stdb01
sudo su -
```

## 2. Check Directory Permissions & Ownership:
Inspect current state.Check the current owner and permissions of /var/lib/mysql:

```Bash
ls -ld /var/lib/mysql
```

Also check if SELinux is actively enforcing policies:

```Bash
ls -Zd /var/lib/mysql
getenforce
```


## 3. Fix Ownership & Permissions Directly:Restore correct permissions.
Grant full ownership of `/var/lib/mysql` and all its contents to the `mysql` user and group:

```Bash
chown -R mysql:mysql /var/lib/mysql
chmod 755 /var/lib/mysql
```

If SELinux is set to Enforcing, restore the default security context on the directory:

```Bash
restorecon -Rv /var/lib/mysql
```

## 4. Start & Enable MariaDB:Start service.Start the MariaDB service and ensure it is set to start on boot:

```Bash
systemctl start mariadb
systemctl enable mariadb
```

## 5. Verify Database Status:Validation.Confirm that the service started successfully and accepts connections:

```Bash
systemctl status mariadb --no-pager
mysql -u root -e "SHOW DATABASES;"
```