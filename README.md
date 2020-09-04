# Enabling Federation to Huawei Cloud Using Windows Active Directory, ADFS, and SAML 2.0

## Introduction to Identity Provider (IdP).
IdP capabilities can be used when an enterprise already has its own authentication system, like Active Directory, and wants to log on to Huawei Cloud using its own authentication system without having to re-create the corresponding IAM user in the Huawei cloud. With this feature, enterprise users do not need to re-create users in the Huawei cloud, only use the username password already in the enterprise to log in and use Huawei cloud resources. With the Identity Provider (IdP) mechanism, you can grant users in your enterprise permission to use your account for cloud resources.

This article uses the enterprise IdP system as an example of Active Directory Federation Services (ADFS) to illustrate how to configure users to access Huawei's cloud systems. Among them, we use Active Directory (AD) as user management and access software.

HUAWEI CLOUD supports two types of federated identity authentication:

- Web SSO: Browsers are used as the communication media. This authentication type enables common users to access HUAWEI CLOUD using browsers.

- API calling: Development tools (such as OpenStack Client and ShibbolethECP Client) are used as the communication media. This authentication type enables enterprise users or common users to access HUAWEI CLOUD by calling APIs.


### ADFS and Huawei Cloud Identity Provider Authentication Process.
The following image shows the joint certification process between ADFS and Huawei Cloud.
![IdP Authentication Process](/img/idp-auth-process.png)

As you can see from the figure, the steps for authentication are:

   1. The user opens a login link from Huawei Cloud in the browser, which initiates a single sign-on request to Huawei Cloud.

   2. Huawei Cloud looks for the corresponding Metadata file based on the information carried in the login link, builds a SAML Request, and sends it to the browser.

   3. After the browser responds, forward SAML Request to ADFS.

   4. The user enters the user name and password created in the AD in the browser to complete the authentication.

   5. The ADFS server builds SAML assertions that carry user information and sends SAML Response to the browser.

   6. The browser responds and forwards SAML Response to Huawei Cloud.

   7. Huawei Cloud takes assertions from SAML Response and maps them to specific IAM user groups based on configured identity conversion rules, issuing Token.

   8. Users complete single sign-on and access Huawei Cloud based on permissions.

## Environment requirements and operating.

1. Requirements:

   - You already have an available Windows computer to install ADFS and have administrator rights for that computer. For hardware requirements for computers for ADFS, see [Deploy ADFS requirements](https://docs.microsoft.com/en-us/previous-versions/azure/azure-services/dn151311(v=azure.100)).

   - The Active Directory domain service is installed on your Windows computer and has added at least two users and groups using the domain service. Two of these users belong to two user groups. For example, user John and Daenerys have been created, user groups HWC-ADFSAdmin and HWC-ADFSGuest, where John belongs to HWC-ADFSADmin and Daenerys belongs to HWC-ADFSGuest. The user group HWC-ADFSAdmin is used to map to the admin user group in the IAM, and the HWC-ADFSGuest is used to map to the guest user group in the IAM.

   - Your Windows computer is connected to internet.

   - You have registered an available account on Huawei Cloud and have created a user group in IAM, such as a guest user group.

   - On Huawei Cloud console created a *guest* user group through IAM. For more details, see [Creating a User Group and Assigning Permissions](https://support.huaweicloud.com/intl/en-us/usermanual-iam/iam_03_0001.html) link.

   - ### Setup instructions:
   You must follow below instructions before continue in this article:
      1. [Install Active Directory Domain Services](Install_ActiveDirectoryServices.md)
      2. [Install Active Directory Certificate Services](Install_ADCertificateServices.md)
      3. [Configure DNS Zones](Config_DNSZone.md)
      4. [Install Active Directory Federation Services](Install_ADFederationServices.md)

Through the operation of this article, HWC-ADFSAdmin created by the AD domain is mapped to the admin user group in huawei's cloud account, and HWC-ADFSGuest is mapped to the guest user group in IAM. After completing all the work in this article, you can successfully log into Huawei Cloud using John or Daenerys, an AD domain user, via a link provided by the identity provider created in IAM. John has admin permissions because it belongs to the HWC-ADFSAdmin group, which maps to the ADmin group of IAM. Daenerys belongs to the HWC-ADFSGuest group, which is mapped to the guest group of IAM, and therefore has the permissions configured by the guest group.

![IdP map to IAM](/img/idp-map-iam.png)


Now that you follow all 4 previous steps, it's time to configure your IdP (ADFS) and Huawei Cloud Identity Provider and test your configurations. Let's start!!!

## Add Huawei cloud trust to ADFS.
After the ADFS installation configuration is complete, the trust relationship of Huawei Cloud needs to be configured into ADFS to complete the addition of the information relationship of ADFS to Huawei Cloud.

1. Save an assertion description of Huawei Cloud. Enter the link below in your browser and save the page as "metadata.xml".
[Huawei Metabase.xml](https://auth-intl.huaweicloud.com/authui/saml/metadata.xml)

2. Open server manager, click Tools, in the drop down list select AD FS Management.
![ADFS Config](/img/adfs-config-01.png)

3. In AD FS Management Tools and click *Required: Add trusted relying party.
![ADFS Config](/img/adfs-config-02.png)

4. In the pop-up window, "Welcome" and click "Start."
![ADFS Config](/img/adfs-config-03.png)

5. In Select Data Source, select Import data about the relying party from a file, click Browse, upload the "metadata.xml" saved in the step 1, and click Next.
![ADFS Config](/img/adfs-config-04.png)

6. Fill in the trusted party display name in Specify Display Name, and click Next.
![ADFS Config](/img/adfs-config-05.png)

7. In Choose access control policy, leave *permite everyone* as the default choice and click Next.
![ADFS Config](/img/adfs-config-06.png)

8. In Ready to add trust, click Next.
![ADFS Config](/img/adfs-config-07.png)

9. Unselect the check box and Click Close.
![ADFS Config](/img/adfs-config-08.png)


## Add Huawei Claims Rules to ADFS relying party.

In order to make our ADFS and Huawei Cloud Identiry Provider to work properly we need to add a set of claim rules. This step is very importat and you need to do the configuration according your own environment and users and groups.
Remember, in this article we set the following:
Users: john, daenerys
Groups: HWC-ADFSAdmin, HWC-ADFSGuest
Where john belongs to HWC-ADFSAdmin and daenerys to HWC-ADFSGuest
So in your own environment use it accordingly.!!!
*Let's go and create our rules.*

In AD FS Management Tools and right click on the newly created relying party and select *Edit claim...*
![ADFS Rules](/img/adfs-config-claim-00.png)

**_Now we need to create 5 rules so our claims and tokens will work properly_**

1. Rule 01: Click Add Rules
![ADFS Rules](/img/adfs-config-claim-01.png)
![ADFS Rules](/img/rule-claims-01.png)
![ADFS Rules](/img/rule-claims-02.png)

Use the following settings:
   1. Claim Rule template: Send Claims Using a Custom Rule 
   2. Claim Rule name: CustomRuleAccount01
   3. Custom Rule: c:[Type =="http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname",Issuer == "AD AUTHORITY"]=> add(store = "ActiveDirectory", types =("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"),query = ";sAMAccountName;{0}", param = c.Value);

2. Rule 02: Click Add Rules  
![ADFS Rules](/img/rule-claims-03.png)
![ADFS Rules](/img/rule-claims-04.png)
![ADFS Rules](/img/rule-claims-05.png)  
  
Use the following settings:
   1. Claim Rule template: Send Claims Using a Custom Rule 
   2. Claim Rule name: CustomRuleAccount02
   3. Custom Rule: c:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"]=> issue(Type ="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier",Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, Value = c.Value,ValueType = c.ValueType, Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/format"]= "urn:oasis:names:tc:SAML:2.0:nameid-format:transient",Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/spnamequalifier"]= "https://auth.huaweicloud.com/");  
  
  
3. Rule 03: Click Add Rules  
![ADFS Rules](/img/rule-claims-06.png)
![ADFS Rules](/img/rule-claims-07.png)
![ADFS Rules](/img/rule-claims-08.png)  
![ADFS Rules](/img/rule-claims-09.png)

Use the following settings:
   1. Claim Rule template: Send Group Membership as a Claims 
   2. Claim Rule name: GroupMemberRuleAdmin
   3. User's Group: MYCOMPANY\HWC-ADFSAdmin
   4. Outgoing Claim Type: Group
   5. Outgoing Claim Value: HWC-ADFSAdmin

4. Rule 04: repeat same steps in the *Rule 03* to add HWC-ADFSGuest as a group membership.
Use the following settings:
   1. Claim Rule template: Send Group Membership as a Claims 
   2. Claim Rule name: GroupMemberRuleGuest
   3. User's Group: MYCOMPANY\HWC-ADFSGuest
   4. Outgoing Claim Type: Group
   5. Outgoing Claim Value: HWC-ADFSGuest

5. Rule 05: Click Add Rules
![ADFS Rules](/img/rule-claims-10.png)
![ADFS Rules](/img/rule-claims-11.png)
![ADFS Rules](/img/rule-claims-12.png)  
![ADFS Rules](/img/rule-claims-fiverules.png)

Use the following settings:
   1. Claim rule name: LDAPNameRule
   2. Attribute store: Active Directory
   3. LDAP Attribute: SAM-Account-Name : Outgoing Claim: Windows account name 
   4. LDAP Attribute: E-Mail-Addresses : Outgoing Claim: E-mail


## Register SPN in PowerShell and turn off authentication extension protection.

1. Open Windows PowerShell as administrator and run the following command to register SPN.

```
setspn -s http/adfs.mycompany.local administrator
```
Result:
PS> setspn -s http/adfs.mycompany.local administrator
Checking domain DC=mycompany,DC=local
Registering ServicePrincipalNames for CN=Administrator,CN=Users,DC=mycompany,DC=local
        http/adfs.mycompany.local
Updated object

2. Because Windows Server uses authentication extension protection, you cannot sign in to cloud services with integrated Windows authentication from within your corporate network because some browsers do not support extended protection. Therefore, in order for ADFS to log on to Huawei Cloud properly, the extended protection of ADFS needs to be turned off.

Run the following command in PowerShell, turn off the extended protection of ADFS, and restart AD FS Management when set up finished.

```
Set-ADFSProperties -ExtendedProtectionTokenCheck None
```
Result: 
WARNING: PS0038: This action requires a restart of the AD FS Windows Service. If you have deployed a federation server farm, restart the service on every server in the farm.

## Enable ADFS SSO page.
By default ADFS Sign on Page is  disabled. We need to enable it first to continue.

1. Open Windows PowerShell as administrator and run the following command:

```
PS> Get-AdfsProperties
...
...
...
EnableIdpInitiatedSignonPage               : False
...
...
...
```
As we can see the properties *EnableIdpInitiatedSignonPage* is false and we need to enable it.

```
PS> Set-AdfsProperties -EnableIdpInitiatedSignonPage $true
Run Get-AdfsProperties again
PS> Get-AdfsProperties
...
...
...
EnableIdpInitiatedSignonPage               : True
...
...
...
```

## Validate ADFS and IAM connection
Verify that the IAM connection was successful. Open the link below in your browser to show the following image of a successful connection.

ADFS Link: https://adfs.hwcping.com.br/adfs/ls/idpinitiatedsignon.aspx
![ADFS Conn](/img/adfs-conn-test-01.png)

## Configure the identity provider on Huawei Cloud IAM.

After the installation and information relationship configuration of ADFS is completed, you will also need to create an ADFS identity provider on Huawei Cloud IAM and add the metadata file of ADFS to establish Huawei Cloud's trust in ADFS. In addition, the mapping relationship between the AD domain user group and the Huawei cloud user group needs to be configured in IAM's identity provider to enable AD domain users to complete joint authentication and permission scope with Huawei cloud through ADFS.

So let's begin our Huawei Cloud IAM Identity Provider configuration. 

1. Log in to your Huawei Cloud Account

2. Open the following link in the browser, where the underlying content is replaced with the CA certificate name and the file will be saved as federationmetadata.xml.
ADFS Metadata Link: https://adfs.hwcping.com.br/federationmetadata/2007-06/federationmetadata.xml

3. On Huawei cloud go to the IAM console, click Identity Provider in the navigation bar on the left side of the console, and click Create Identity Provider.
![HwC IdP Config](/img/hwc-idp-config-01.png)

4. On the Create Identity Provider page, fill in the identity provider name, agreement, status, and click OK.
![HwC IdP Config](/img/hwc-idp-config-02.png)

5. Click Modify after the identity provider you have created.
![HwC IdP Config](/img/hwc-idp-config-03.png)

6. In The Metadata Configuration on the Modify Identity Provider page, upload the downloaded federmetadata.xml from step #2.
![HwC IdP Config](/img/hwc-idp-config-04.png)

7. Click Edit Rules to fill in the following rules in the Identity Conversion Rules on the Modify Identity Providers page. For a detailed description of IAM rules, see the [Federal User Identity Rule Conversion Instructions](https://support.huaweicloud.com/intl/en-us/usermanual-iam/iam_08_0006.html).
![HwC IdP Config](/img/hwc-idp-config-05.png)

The following rules indicate that the HWC-ADFSAdmin group for the AD domain maps to the Admin group for the IAM. and HWC-AdFSGuest group maps to the IAM's guest group. And after all users in the user group login in the Huawei cloud, the IAM user name displayed on the Huawei cloud is "mycompany_ userna name", for example, the IAM user name displayed after Daenarys logs on is "mycompany_Daenerys".

My Syntax of Identity Conversion Rules code:

    [{
    	"remote": [{
    		"type": "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
    	}, {
    		"any_one_of": ["HWC-ADFSAdmin"],
    		"type": "http://schemas.xmlsoap.org/claims/Group"
    	}],
    	"local": [{
    		"user": {
    			"name": "mycompany_{0} "
    		}
    	}, {
    		"group": {
    			"name": "admin"
    		}
    	}]
    }, {
    	"remote": [{
    		"type": "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
    	}, {
    		"any_one_of": ["HWC-ADFSGuest"],
    		"type": "http://schemas.xmlsoap.org/claims/Group"
    	}],
    	"local": [{
    		"user": {
    			"name": "mycompany_{0} "
    		}
    	}, {
    		"group": {
    			"name": "guest"
    		}
    	}]
    }]

Copy the exactly code above to your Identity Conversion Rules
![HwC IdP Config](/img/hwc-idp-config-06.png)

Click validate to parse your JSON code. If the validation process is sucessful, click Ok to finish:
![HwC IdP Config](/img/hwc-idp-config-06a.png)

8. On the Modify Identity Provider page, copy the login link and enter the link below in your browser.
![HwC IdP Config](/img/hwc-idp-config-07.png)

9. The browser pops up on the following page, fills in the username and password of the AD domain, clicks "Login" and successfully logs on to the Huawei cloud console, indicating that the domain user in the AD has successfully logged to Huawei Cloud.


10. The following page appears to indicate that Alice, a user using the AD domain, successfully logged into Huawei Cloud.
