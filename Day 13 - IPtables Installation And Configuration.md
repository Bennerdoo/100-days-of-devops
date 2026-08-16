# Question

We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 6000 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:


1. Install iptables and all its dependencies on each app host.

2. Block incoming port 6000 on all apps for everyone except for LBR host.

3. Make sure the rules remain, even after system reboot.

# Step-by-Step Solution(Perform on each App Server)

## 1. SSH into the Target App Server

**Connect from jump host:**Connect to the target application server from the jump host:

```Bash
ssh tony@stapp01   # For stapp01
# ssh steve@stapp02  (for stapp02)
# ssh banner@stapp03 (for stapp03)
```

## 2. Install iptables Package

**Install iptables-services:**

Install iptables and iptables-services using yum or dnf:

```Bash
sudo yum install -y iptables iptables-services
```

## 3. Add iptables Rules for Port 6000

**Configure firewall rules:**
Add the rule to ALLOW traffic on TCP port 6000 from the Load Balancer host (stlb01), followed by the rule to DROP all other incoming traffic on port 6000:

```Bash
# Allow port 6000 traffic from LBR host
sudo iptables -A INPUT -p tcp -s stlb01 --dport 6000 -j ACCEPT

# Drop all other incoming traffic on port 6000
sudo iptables -A INPUT -p tcp --dport 6000 -j DROP
```

>Rule order is critical: the ACCEPT rule must be placed before the DROP rule in the INPUT chain.

## 4. Save Rules to Configuration File

**Bypasses missing 'service' binary.Save the active firewall configuration directly to /etc/sysconfig/iptables so it survives system restarts:**

```Bash
sudo iptables-save | sudo tee /etc/sysconfig/iptables
```

## 5. Enable and Start iptables Service

**Enable systemd daemon.Enable and start the systemd service to ensure the saved rules automatically load at boot time:**

```Bash
sudo systemctl enable --now iptables
```

## 6. Verify Rules and Service Status

**Verification.Confirm that the service is running and that the rules are correctly ordered:**

```Bash
sudo systemctl status iptables --no-pager
sudo iptables -L INPUT -n -v --line-numbers
```

**Expected Output:**
```Bash
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 ACCEPT     tcp  --  *      *       172.16.238.14        0.0.0.0/0            tcp dpt:6000
2        0     0 DROP       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:6000
```
