# Question

The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:


PostgreSQL database server is already installed on the Nautilus database server.

- a. Create a database user kodekloud_tim and set its password to YchZHRcLkL.

- b. Create a database kodekloud_db3 and grant full permissions to user kodekloud_tim on this database.

Note: Please do not try to restart PostgreSQL server service.

# Step by Step Solution

1. **SSH into Database Server:**
Connect from jump host.Connect to stdb01 from the jump host as user peter:Bashssh peter@stdb01

2. **Connect to PostgreSQL shell as postgres user:**
PostgreSQL CLI.Switch to the postgres system user and open the interactive psql console:

```Bash
sudo -u postgres psql
```

3. **Create User, Database, and Grant Permissions:**
SQL commands.Execute the following SQL queries inside the psql prompt:

```SQL
-- Create the database user with password
CREATE USER kodekloud_tim WITH PASSWORD 'YchZHRcLkL';

-- Create the database owned by the user
CREATE DATABASE kodekloud_db3 OWNER kodekloud_tim;

-- Grant all privileges on the database
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db3 TO kodekloud_tim;
```

4. **Exit the PostgreSQL Shell:**
Exit psql.Exit psql by typing:

```SQL
\q
```

5. **Verify Database and User Creation:**
Validation.Test connecting to the newly created database using the new user credentials:

```Bash
PGPASSWORD='YchZHRcLkL' psql -U kodekloud_tim -d kodekloud_db3 -h localhost
Once connected, type \q to exit.