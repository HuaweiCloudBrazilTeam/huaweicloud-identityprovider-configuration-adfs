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

   - The Active Directory domain service is installed on your Windows computer and has added at least two users and groups of users using the domain service. Two of these users belong to two user groups. For example, user John and Daenerys have been created, user groups HWC-ADFSAdmin and HWC-ADFSGuest, where John belongs to HWC-ADFSADmin and Daenerys belongs to HWC-ADFSGuest. The user group HWC-ADFSAdmin is used to map to the admin user group in the IAM, and the HWC-ADFSGuest is used to map to the guest user group in the IAM.

   - Your Windows computer is connected to Huawei's cloud network.

   - You have registered an available account on Huawei Cloud and have created a user group in IAM, such as a guest user group.

2. Example of environment and operating systems.
   - The computer and software specifications used in this article are described below, and the next steps are based on this environment.

   - Computer: The operating system used to install ADFS is the Windows 2016 R2 64-bit version created by Huawei Cloud Elastic Cloud Server (ECS)

   - Active Directory (AD) domain services have been installed in elastic cloud servers and HWC-ADFSAdmin, HWC-ADFSGuest user groups, John and Daenerys users have been created, with John belonging to the HWC-ADFSAdmin user group and Daenerys belonging to the HWC-ADFSGuest user group.

On Huawei Cloud console created a *guest* user group through IAM. For more details, see [Creating a User Group and Assigning Permissions](https://support.huaweicloud.com/intl/en-us/usermanual-iam/iam_03_0001.html) link.

Through the operation of this article, HWC-ADFSAdmin created by the AD domain is mapped to the admin user group in huawei's cloud account, and HWC-ADFSGuest is mapped to the guest user group in IAM. After completing all the work in this article, you can successfully log into Huawei Cloud using John or Daenerys, an AD domain user, via a link provided by the identity provider created in IAM. John has admin permissions because it belongs to the HWC-ADFSAdmin group, which maps to the ADmin group of IAM. Daenerys belongs to the HWC-ADFSGuest group, which is mapped to the guest group of IAM, and therefore has the permissions configured by the guest group.

![IdP map to IAM](/img/idp-map-iam.png)