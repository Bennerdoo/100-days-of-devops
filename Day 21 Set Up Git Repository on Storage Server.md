# Question

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:


- Utilize yum to install the git package on the Storage Server.

- Create a bare repository named /opt/demo.git (ensure exact name usage).

# Step-by-Step Solution

1. **SSH into Storage Server:**
Connect from jump host.Connect to ststor01 from the jump host as user natasha:
```Bash
ssh natasha@ststor01
```

2. **Install Git via yum:**
Package installation.Install the git package using yum:
```Bash
sudo yum install -y git
```

3. **Create the Bare Git Repository:**
Bare repo initialization.Initialize a bare repository at /opt/demo.git:
```Bash
sudo git init --bare /opt/demo.git
```

4. **Adjust Repository Permissions:**
Permissions management.Ensure proper permissions are assigned so users can read and write to the repository:
```Bash
sudo chmod -R 777 /opt/demo.git
```

5. **Verify Bare Repository Setup:**
Validation.Verify that the bare repository directory structure has been created properly:
```Bash
ls -la /opt/demo.git
```


**Expected Output:** Contains essential Git files and directories such as HEAD, config, description, hooks/, info/, objects/, and refs/.