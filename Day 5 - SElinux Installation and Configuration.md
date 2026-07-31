# Question

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 1 in the Stratos Datacenter:


Install the required SELinux packages.

Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.

No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

# Step-by-step Solution

1. SSH into App Server 1:
Jump host to stapp01.Log into App Server 1 using your user account (typically tony on stapp01):
```Bash
ssh tony@stapp01
```

2. Install required SELinux packages:Requires sudo.Install the core SELinux management and utility packages using yum or dnf:
```Bash
sudo yum install -y selinux-policy selinux-policy-targeted policycoreutils libselinux-utils
```

3. Permanently disable SELinux in configuration:
Edit **etc/selinux/config**.Update the main SELinux configuration file so that it remains disabled after the upcoming maintenance reboot.Open the file using sudo vi /etc/selinux/config (or /etc/sysconfig/selinux):Set the SELINUX directive to disabled:
```Plaintext
SELINUX=disabled
SELINUXTYPE=targeted
```
Alternatively, apply the update using a one-liner:
```Bash
sudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

4. Confirm SELinux config setting:Verify persistent config.Check that the persistent file correctly specifies SELINUX=disabled:Bash
```Bash
grep '^SELINUX=' /etc/selinux/config
```
Expected output:
```Plaintext
SELINUX=disabled
```

> **Note:** As specified in the requirements, you do not need to run setenforce 0 or reboot the server now. Modifying **/etc/selinux/config** guarantees that SELinux will be permanently disabled upon the scheduled reboot tonight.