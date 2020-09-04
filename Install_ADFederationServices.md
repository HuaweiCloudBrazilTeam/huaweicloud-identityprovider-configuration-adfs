# Install Active Directory Federation Service

This section describes how to install ADFS on a Windows 2016, make sure that you have completed the addition of the [Install Active Directory Services](Install_ActiveDirectoryServices.md) and  [Intall AD Certificate Services](Install_ADCertificateServices.md).

1. Login to your ECS created to run the ADFS.

2. Now we need to request a certificate to use in our ADFS setup
   1. Click start --> type: *MMC* and then hit enter
   2. You will see a window like the image below
   ![MMC](/img/certificate-01.png)
   3. Now click in *file* and them select *add or remove snap-in*
   ![MMC](/img/certificate-02.png)
   4. Add the certificates snap-in following images below 
   ![MMC](/img/certificate-03.png)
   ![MMC](/img/certificate-04.png)
   ![MMC](/img/certificate-05.png)
   ![MMC](/img/certificate-06.png)
   5. Now lets request a new certificate based on the template we created in the [Intall AD Certificate Services](Install_ADCertificateServices.md)
   ![MMC](/img/certificate-07.png)
   6. Right click on the *personal folder* --> *all tasks* --> *Request a new certificate...*
   ![MMC](/img/certificate-08.png)
   7. In the Welcome window click next.
   ![MMC](/img/certificate-09.png)
   8. In the *Select certificate...* page click next
   ![MMC](/img/certificate-10.png)
   9. In the *Request Certificate*, click in the option *More information is required...* to configure your certificate.
   ![MMC](/img/certificate-11.png)
   10. a
   ![MMC](/img/certificate-12.png)
   ![MMC](/img/certificate-13.png)
   ![MMC](/img/certificate-14.png)
   ![MMC](/img/certificate-15.png)
   11. Now that you finish your certificate configuration, click Enroll.
   ![MMC](/img/certificate-16.png)
   ![MMC](/img/certificate-17.png)
   ![MMC](/img/certificate-18.png)



3. Open server manager, click Add roles and features, in the Add Roles and Features Wizard that bounces out, Install Type select Role-based or Feature-Based Installation by default, and click Next.
![Server Manager](/img/servermanager.png)

4. In the pop-out window , Before you begin, Installation Type and Sever Selection, click Next.
![ADFS Install](/img/adfs-install-01.png)
![ADFS Install](/img/adfs-install-02.png)
![ADFS Install](/img/adfs-install-03.png)

5. Server Roles check Active Directory Federation Services and click Next.
![ADFS Install](/img/adfs-install-04.png)

6. Features and AD FS click Next by default.
![ADFS Install](/img/adfs-install-05.png)

7. Confirm, click Install, and start installing ADFS.
![ADFS Install](/img/adfs-install-06.png)
![ADFS Install](/img/adfs-install-07.png)

8. When the installation is complete, click the red box in the image below to begin the configuration of the ADFS.
![ADFS Install](/img/adfs-install-08.png)

9. In the pop-up "Welcome" page, select *create the first federation...* and click Next.
![ADFS Install](/img/adfs-install-09.png)

10. In Connect to AD DS, click Next.
![ADFS Install](/img/adfs-install-10.png)

11. On the Specify Service Properties page, select the SSL certificate you created earlier and fill in the display name.
- Description: SSL certificate Please select the computer name and domain name, for example, select adfs.hwcping.com.br
![ADFS Install](/img/adfs-install-11.png)

- The Federation Service Name, which is the domain that accesses the Federation Service, records that you need to use in obtaining the Federation Service profile.

- The Federation Service Display Name, the name that appears in the browser interface when the user's browser verifies its identity.

12. Use an existing domain management account in The Specified Service Account.
![ADFS Install](/img/adfs-install-12.png)
![ADFS Install](/img/adfs-install-13.png)
![ADFS Install](/img/adfs-install-14.png)

13. Click Next and leave the default option in Specify Database.
![ADFS Install](/img/adfs-install-15.png)

14. Click Next in View Selection. *optional*: you can save Powershell script if you want.
![ADFS Install](/img/adfs-install-16.png)

15. In Prerequisite Check, click Configure.
![ADFS Install](/img/adfs-install-17.png)

16. When you see "This server was successfully configured," the configuration is successful.
![ADFS Install](/img/adfs-install-18.png)


## Finished ADFS instalation.
