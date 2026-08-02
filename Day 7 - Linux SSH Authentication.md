# Question

The system admins team of xFusionCorp Industries has set up some scripts on jump host that run on regular intervals and perform operations on all app servers in Stratos Datacenter. To make these scripts work properly we need to make sure the thor user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e tony for app server 1). Based on the requirements, perform the following:


- Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.

# Step-by-Step Solution

1. **Generate SSH Key Pair on Jump Host:**
Executed as thor on jump host.Make sure you are logged in as the thor user on the jump host. Generate an SSH key pair (press Enter for all prompts to leave passphrase empty):Bashssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa

2. **Copy Public Key to App Servers:**
Copy public key to App Servers.Copy the SSH public key from thor to each app server's respective sudo user:

**App Server 1 (stapp01) — User: tony**

```Bash
ssh-copy-id tony@stapp01
```

**App Server 2 (stapp02) — User: steve**

```Bash
ssh-copy-id steve@stapp02
```

**App Server 3 (stapp03) — User: banner**
```Bash
ssh-copy-id banner@stapp03
```

(Enter the password for each user when prompted during key deployment.)
3. **Test Password-less SSH Connections:**
Verify login without password.Verify that you can SSH into each server without being prompted for a password:
```Bash
ssh tony@stapp01 "hostname"
```
```Bash
ssh steve@stapp02 "hostname"
```

```Bash
ssh banner@stapp03 "hostname"
```

**Expected output:**

Each command should instantly return the hostname of the respective server (stapp01, stapp02, stapp03) without asking for a password.