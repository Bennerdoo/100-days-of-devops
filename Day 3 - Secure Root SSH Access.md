# Question

Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.


Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.


# Step-by-step solution (Do in all 3 app servers)

1. SSH into App Server 1:Jump host to stapp01.Log into App Server 1 using your assigned credentials (typically user tony on stapp01)
```Bash
ssh tony@stapp01
```
2. Edit SSH configuration file:Requires sudo.Open the sshd_config file using a text editor (vi or nano):
```Bash
sudo vi /etc/ssh/sshd_config
```
3. Disable direct root login:Locate the line containing 'PermitRootLogin'.If it is set to 'yes' or commented out, change it to 'no':
```Bash
PermitRootLogin no
```
4. Restart SSH service:Apply changes.Save the file and restart the SSH service to make the changes effective:
```Bash
sudo systemctl restart sshd
```
5. Verify root login restriction:Test the change.Try logging in as root again; it should now fail, forcing the use of sudo or su after logging in as a regular user.
> Note: Always ensure you have sudo privileges on the target server. After disabling root login, use sudo su - or sudo -i for elevated access.
- **Tip**: The PermitRootLogin no directive prevents direct SSH access for the root account, enforcing a more secure workflow where administrative tasks are performed by regular users with explicit sudo permissions.

# Alternative solution
## One-Liner Alternative (Per Server)
If you prefer to automate this step across all three servers, you can run this sed command followed by restarting the service:

```Bash


sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config && sudo sshd -t && sudo systemctl restart sshd
```
> Note: Also check if any drop-in configuration files exist under /etc/ssh/sshd_config.d/ that might override the main file. You can verify the active configuration using :
```Bash
sudo sshd -T | grep permitrootlogin.