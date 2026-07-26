# Day 1 - Linux User Setup with Non-Interactive Shell

## Question

To accommodate the backup agent tool's specifications, the system admin team at `xFusionCorp Industries` requires the creation of a user with a non-interactive shell. Here's your task:

Create a user named `mark` with a non-interactive shell on `App Server 2`.

> Note: You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

## Step-by-Step Solution

### 1. SSH into App Server 2

Jump host to `stapp02`. Log in to App Server 2 using the credentials provided in the task details:

```bash
ssh steve@stapp02
```

### 2. Create the user with a non-interactive shell

Use `sudo` and `useradd` with the `-s` flag to set the login shell to `/sbin/nologin`:

```bash
sudo useradd -s /sbin/nologin mark
```

### 3. Verify the user and shell

Check `/etc/passwd` to confirm that the user `mark` was created with the non-interactive shell:

```bash
grep mark /etc/passwd
```

Expected output:

```text
mark:x:1001:1001::/home/mark:/sbin/nologin
```

> Why `/sbin/nologin`? Setting a user's shell to `/sbin/nologin` prevents interactive login attempts via SSH or terminal while still allowing backup agents, daemon processes, or system tasks to run under that user identity.

