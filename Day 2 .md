# Question

As part of the temporary assignment to the Nautilus project, a developer named jim requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:

- Create a user named jim on App Server 1 in Stratos Datacenter. 
- Set the expiry date to 2027-02-17, ensuring the user is created in lowercase as per standard protocol.

Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

# Step-by-step Solution

1.SSH into App Server 1:Jump host to stapp01.Log into App Server 1 using your assigned credentials (typically user tony on stapp01)```Bash
ssh tony@stapp01
```
2.Create user with expiration date:Requires sudo.Use the useradd command with the -e (or --expiredate) option set to 2027-02-17:
```Bash
sudo useradd -e 2027-02-17 jim
```
3.Verify account expiration details:Check aging info.Confirm that the account creation and expiration date were applied correctly:
```Bash
sudo chage -l jim
```
>Expected output line:PlaintextAccount expires : Feb 17, 2027
- **Tip**: The YYYY-MM-DD date format used with useradd -e converts automatically into the system's standard epoch time, disabling jim's account automatically at midnight on February 17, 2027.