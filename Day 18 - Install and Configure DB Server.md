# Question

We need to setup a database server on Nautilus DB Server in Stratos Datacenter. Please perform the below given steps on DB Server:


- a. Install/Configure MariaDB server.

- b. Create a database named kodekloud_db7.

- c. Create a user called kodekloud_pop and set its password to TmPcZjtRQx.

- d. Grant full permissions to user kodekloud_pop on database kodekloud_db7.

# Step by Step Solution

## Step 1: SSH into Database Server:
Connect from jump host.

```Bash
ssh peter@stdb01
```

## Step 2: Install MariaDB Server:
Package installation.

```Bash
sudo yum install -y mariadb-server mariadb
```

## Step 3: Start and Enable MariaDB Service:
Service management.

```Bash
sudo systemctl enable --now mariadb
sudo systemctl status mariadb --no-pager
```

## Step 4: Create Database, User, and Grant Privileges:

SQL commands.Execute the following SQL commands directly via mysql as root:

```Bash
sudo mysql -e "CREATE DATABASE IF NOT EXISTS kodekloud_db7;"
sudo mysql -e "CREATE USER IF NOT EXISTS 'kodekloud_pop'@'%' IDENTIFIED BY 'TmPcZjtRQx';"
sudo mysql -e "GRANT ALL PRIVILEGES ON kodekloud_db7.* TO 'kodekloud_pop'@'%';"
sudo mysql -e "FLUSH PRIVILEGES;"
```

Note: Also create the user bound to localhost to allow local command-line access:

```Bash
sudo mysql -e "CREATE USER IF NOT EXISTS 'kodekloud_pop'@'localhost' IDENTIFIED BY 'TmPcZjtRQx';"
sudo mysql -e "GRANT ALL PRIVILEGES ON kodekloud_db7.* TO 'kodekloud_pop'@'localhost';"
sudo mysql -e "FLUSH PRIVILEGES;"
```

## Step 5: Verify Access:

Validation.

Test connecting to the new database with the created credentials:

```Bash
mysql -u kodekloud_pop -p'TmPcZjtRQx' kodekloud_db7 -e "SHOW TABLES;"
```

Expected Output: Executes without access denied errors.