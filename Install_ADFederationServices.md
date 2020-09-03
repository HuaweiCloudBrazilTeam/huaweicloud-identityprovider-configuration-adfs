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

4. In the pop-out window , Install Type selects Role-Based or Feature-Based Installation by default, click Next.
![ADFS Install](adfs-install-01.png)

