# Install Active Directory Certificate (CA) Service

Because the certificate service is required to install ADFS, you need to add the certificate service to your computer first. If you have a certificate service installed, skip this section.

1. Login to your ECS created to run the Active Directory.

2. Open server manager, click Add roles and features, in the Add Roles and Features Wizard that bounces out, Install Type select Role-based or Feature-Based Installation by default, and click Next.
![Server Manager](/img/servermanager.png)

3. In the following wizard pages click Next.
![Wizzard 01](/img/add-features-wiz-01.png)
![Wizzard 01](/img/add-features-wiz-02.png)
![Wizzard 01](/img/add-features-wiz-03.png)

4. In the Server Role, confirm that Active Directory Certificate Service is checked, if it is not checked to indicate that it is not installed, check Active Directory, select by default in the pop-up window, and click Add Features to complete the installation of the Certificate Service.
![ADCS Install](img/adcs-serverroles01.png)
![ADCS Install](img/adcs-serverroles02.png)
![ADCS Install](img/adcs-serverroles03.png)
![ADCS Install](img/adcs-serverroles04.png)

5. In Role Services, select Certificate Authority by default and click Next.
![ADCS Install](img/adcs-serverroles05.png)
![ADCS Install](img/adcs-serverroles06.png)

6. In Confirm page, click Install.
![ADCS Install](img/adcs-serverroles07.png)

7. When the installation is complete click *Configure Active Directory Certificate Services on the destination server*
![ADCS Install](img/adcs-serverroles08.png)
![ADCS Install](img/adcs-serverroles09.png)

8. Click Next in Credentials page in the AD CS Configuration window.
![ADCS Install](img/adcs-serverroles10.png)

9. Select Certification Authority in Role Services and click Next.
![ADCS Install](img/adcs-serverroles11.png)

10. In Setup type, select Enterprise CA, click Next.
![ADCS Install](img/adcs-serverroles12.png)

11. Select Root CA in CA Type and click Next.
![ADCS Install](img/adcs-serverroles13.png)

12. Select Create a new private key in Private Key page and click Next.
![ADCS Install](img/adcs-serverroles14.png)

13. In the Encryptation page, select your encryptation level click Next.
![ADCS Install](img/adcs-serverroles15.png)

14. In CA Name and Valid, use the default value, click Next.
![ADCS Install](img/adcs-serverroles16.png)
![ADCS Install](img/adcs-serverroles17.png)

15. Certificate Database are all selected by default, and click Next.
![ADCS Install](img/adcs-serverroles18.png)

16. In Confirm, click Confirm to complete the certificate configuration.
![ADCS Install](img/adcs-serverroles19.png)

17. When the configuration is successful, the configuration is prompted to be successful, as shown in the following image. Restart your computer for the configured certificate to take effect.
![ADCS Install](img/adcs-serverroles20.png)

# Configure ADFS Certificate Template

1. Open Certificate Authority
![CA Template](img/ca-template01.png)

2. Go to certificate template folder, right click and click manage
![CA Template](img/ca-template02.png)

3. In the new window navigate to the *Web Server* certificate template, rigth click and click *duplicate template*
![CA Template](img/ca-template03.png)

4. Make sure you do the same modification as the pictures below
![CA Template](img/ca-template04.png)
![CA Template](img/ca-template05.png)
![CA Template](img/ca-template06.png)

5. Now create a new template certificate to issue. Right click on certificate template folder and click new --> *Certificate Template to Issue*
![CA Template](img/ca-template07.png)

6. You now can see your newly created certificate template
![CA Template](img/ca-template08.png)


## Finish ADCS instalation







