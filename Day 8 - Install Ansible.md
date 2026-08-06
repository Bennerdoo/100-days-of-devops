# Question

During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with Ansible for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use jump host as an Ansible controller to test different kind of tasks on rest of the servers.


- Install ansible version 4.10.0 on Jump host using pip3 only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

# Step-by-step Solution

Execute the following steps on jump host  (userthor):

1. Install ansible version 4.10.0 using pip3:

Install the correct Ansible version system-wide using pip3. This ensures all users on jump host can run ansible commands.

```Bash
sudo pip3 install ansible==4.10.0
```

2. Verify Ansible installation:
Confirm Ansible is installed and globally accessible.

```Bash
which ansible
```

Expected output (or similar):
```Plaintext
/usr/local/bin/ansible
```


```Bash
ansible --version
```

Expected output (or similar):
```Plaintext
ansible [core 4.10.0]
...
ansible python module location = /usr/local/lib/python3.x/dist-packages/ansible
...
```


> **Tip**: Using pip3 install --user ansible==4.10.0 would install Ansible only for the current user. To make Ansible available globally (system-wide) so that all users can run ansible commands as required, the sudo pip3 install command above is the correct approach, placing the binary in a standard system path like /usr/local/bin.
