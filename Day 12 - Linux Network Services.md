# Question

Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 6200 (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.


Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command `curl http://stapp02:6200 `command from jump host.
**Note:** ***Please do not try to alter the existing index.html code, as it will lead to task failure.***

# Step-By-Step Solution

## Perform These in all the app servers

###  SSH to the application host:

```bash
ssh tony@stapp01
```
### Install the tools you will use:
```bash
sudo yum install -y net-tools
sudo yum install -y iptables
```

###  Check the httpd service status:

```bash
sudo systemctl status httpd

```
### Stop any conflicting processes- Sendmail in this Case

```bash
Sudo systemctl stop sendmail
```

###  Inspect Apache Listen directives (look for duplicates or wrong ports):

```bash
sudo grep -n 'Listen' /etc/httpd/conf/httpd.conf
grep -R 'Listen' /etc/httpd/conf.d/
```


###  Restart Apache and verify it's listening on port 6200
```bash
sudo systemctl restart httpd
sudo systemctl status httpd
ss -tuln | grep 6200
# or, if netstat is available:
sudo netstat -tuln | grep 6200
```

###  Check the host firewall (iptables):

```bash
sudo iptables -L -n --line-numbers
```

If INPUT doesn't allow port 6200, add a rule near the top:

```bash
sudo iptables -I INPUT 1 -p tcp --dport 6200 -j ACCEPT
sudo iptables -L -n --line-numbers
```

Persist rules per your environment, e.g. `service iptables save` or `iptables-save > /etc/sysconfig/iptables` if required.

###  Final checks on the app host:

```bash
ss -tuln | grep 6200
sudo systemctl status httpd
```
###  From the jump host verify connectivity and fetch the page:

```bash
telnet stapp01 6200
curl http://stapp01:6200
```
