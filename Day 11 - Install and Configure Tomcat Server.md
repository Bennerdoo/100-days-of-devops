# Question

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:


- a. Install tomcat server on App Server 1.

- b. Configure it to run on port 6200.

- c. There is a ROOT.war file on Jump host at location /tmp.

Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e curl http://stapp01:6200

# Step-by-Step Solution
### 1. SSH into App Server 1:
Connect from jump host.Log into stapp01 as user tony:
```Bash
ssh tony@stapp01
```
### 2. Install Tomcat:
Requires sudo.Install Apache Tomcat and required dependencies:Bashsudo yum install -y tomcat
```Bash
sudo yum install -y tomcat
```
### 3. Configure Tomcat Port to 6200:
Edit server.xml.Open the main Tomcat configuration file:
```Bash
sudo vi /usr/share/tomcat/conf/server.xml
```
Locate the <Connector> section for HTTP/1.1 (default port 8080) and change port="8080" to port="6200":
```xml

```

>**(Alternatively, use sed to update it directly: `sudo sed -i 's/port="8080"/port="6200"/' /usr/share/tomcat/conf/server.xml`)**
### 4. Deploy ROOT.war File:
Transfer from Jump Host.From the Jump Host (or from stapp01 using scp), copy the ROOT.war file from /tmp into Tomcat's webapps directory on stapp01:
```Bash
scp /tmp/ROOT.war tony@stapp01:/tmp/
```
On stapp01, move ROOT.war to Tomcat's deployment folder and set proper ownership:
```Bash
sudo mv /tmp/ROOT.war /var/lib/tomcat/webapps/ROOT.war
sudo chown tomcat:tomcat /var/lib/tomcat/webapps/ROOT.war
```
### 5. Start and Enable Tomcat:
Start service.Start the Tomcat service and enable it on boot:
```Bash
sudo systemctl enable --now tomcat
```
### 6. Verify Base URL Connectivity:
Curl test.Test that the application responds directly on port 6200:
```Bash
curl http://stapp01:6200
```
**Expected Result:** The command returns the HTML/text response served by the deployed ROOT.war web application.