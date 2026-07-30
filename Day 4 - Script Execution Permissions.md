# Question

In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named xfusioncorp.sh. While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 3 within the Stratos Datacenter.


Your task is to grant executable permissions to the /tmp/xfusioncorp.sh script on App Server 3. Additionally, ensure that all users have the capability to execute it.

# Step-by-Step Solution

1. SSH into App Server 3:Jump host to stapp03.Log into App Server 3 using your assigned credentials (typically user tony on stapp03):
```Bash
ssh banner@stapp03
```
2. Verify current permissions:Check the existing permissions of the script using ls -l:
```Bash
ls -l /tmp/xfusioncorp.sh
```
> Typically, the output will show permissions like -rw-r--r--, indicating read and write permissions but no execute permission for any user group. You will also notice the script exists in /tmp directory but not in /opt/scripts directory.

3. Grant executable permissions:Use chmod to add executable permission for all users (owner, group, and others):
```Bash
sudo chmod 755 /tmp/xfusioncorp.sh
```

4. Verify permissions again:Confirm that the changes have been applied:
```Bash
ls -l /tmp/xfusioncorp.sh
```
> The output should now show an x in the permission string, for example: -rwxr-xr-x.

5. Move script to /opt/scripts directory:Copy the script to its intended location:
```Bash
sudo cp /tmp/xfusioncorp.sh /opt/scripts/
```
6. Verify the script is in /opt/scripts:
```Bash
ls -l /opt/scripts/xfusioncorp.sh
```
> The script is now executable by all users and located in the appropriate directory for scheduled tasks.
