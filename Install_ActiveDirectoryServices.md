# Install Active Directory Domain Service

1. Login to your ECS created to run the Active Directory.

2. Open server manager, click Add roles and features, in the Add Roles and Features Wizard that bounces out, Install Type select Role-based or Feature-Based Installation by default, and click Next.
![Server Manager](/img/servermanager.png)

3. In the following wizard pages click Next.
![Wizzard 01](/img/add-features-wiz-01.png)
![Wizzard 01](/img/add-features-wiz-02.png)
![Wizzard 01](/img/add-features-wiz-03.png)
   
4. Install ADDS & DNS Server Roles:
   * In the Server Role to see if the DNS server is installed, if not, check DNS Server, click Add Features by default in the pop-up window, and install DNS Server.
![DNS Server](/img/selectserverrole-dnsfeatures.png) 

   * Then check Active Directory Domain Services in Server Roles, and click Add Features by default in the pop-up window to install active Directory Domain Services.
![AD Server](/img/selectserverrole-domainfeatures.png)
    
   * In the following wizard page click Next.  
![AD Server](/img/selectserverrole-domainfeatures-next.png)

5. In the following wizard pages click Next...

6. In the Confirm page click Install.
![Install AD](img/install-ad.png)

7. When the installation is complete, click the *Promte this server to the Domain Controller*.
![Promote AD](/img/install-success.png)

## Promote Active Directory

1. In the pop-up window, Deploy Configuration select Add New Forest and fill in the root domain name, click Next. The new forest added in the example here is "mycompany.local".
![Promote AD](/img/promote-ad-01.png)

2. Fill in the custom password in Domain Controller Selection and click Next.
![Promote AD](/img/promote-ad-02.png)

3. Click Next in DNS Options.
![Promote AD](/img/promote-ad-03.png)

4. In Other Options, click Next.
![Promote AD](/img/promote-ad-04.png)

5. Leave the default in Path, and click Next.
![Promote AD](/img/promote-ad-05.png)

6. In review Options, view and confirm the information, and click Next *Save Powershell script if you want to automate this taks for another environment*
![Promote AD](/img/promote-ad-06.png)

7. Click Install in Option Condition Check.
![Promote AD](/img/promote-ad-07.png)

8. When the installation is complete, the system will automatically restart so that the Active Directory installation configuration takes effect.
![Promote AD](/img/promote-ad-08.png)

## Create users and groups.

1. Create two user groups, HWC-ADFSAdmin and HWC-ADFSGuest. Open the Active Directory Management Center with Management Tools.
![ADUC Group](/img/aduc-group.png)
![ADUC Group](/img/aduc-group-01.png)

2. Create two user, John and Daenerys.
![ADUC Group](/img/aduc-user.png)
![ADUC Group](/img/aduc-user01.png)

3. Add users to the respective groups according to the requirements described in this [article](README.md).
![ADUC Group](/img/aduc-group-02.png)


## Finish Active Directory installation