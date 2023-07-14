## Configuring SPF and DKIM Records for Your Linode Ubuntu Server with iRedMail

### Overview

This guide will walk you through the steps to configure SPF and DKIM records for your Linode Ubuntu Server that is running iRedMail. SPF and DKIM records help prevent email spoofing and improve email deliverability by verifying that emails sent from your domain are legitimate. 

### Prerequisites

- A Linode Ubuntu Server with iRedMail installed and configured as a mail server.
- Access to your Linode account and the Linode dashboard.

### Steps

1. Log in to your Linode account and select the Linode instance that you've configured as a mail server with iRedMail.
2. Click on the "Networking" tab and select "DNS Manager" from the drop-down menu.
3. Click on the "Add a Domain Zone" button and enter your domain name in the "Domain" field. Then click on the "Add Zone" button to create the DNS zone for your domain.
4. Once the DNS zone is created, click on the "Add a Record" button to add the SPF record for your domain. 
   - In the "Add a Record" window, select "TXT" from the "Record Type" drop-down menu. In the "Hostname" field, enter "@".
   - In the "Text Data" field, enter the following SPF record:

   ```
   v=spf1 a mx ip4:<your server IP address> ~all
   `````

   Replace "<your server IP address>" with the IP address of your Linode server. 
5. Click on the "Save" button to add the SPF record to your DNS zone.
6. Generate the DKIM key-pair for your domain. Connect to your server using SSH and run the following commands:

   ```
   sudo amavisd-new genrsa /etc/dkim.key 2048
   sudo amavisd-new genrsa /etc/dkim.pub 2048
   sudo chown amavis:amavis /etc/dkim.*
   ````

   This will generate two files, /etc/dkim.key and /etc/dkim.pub, which contain the private and public keys for DKIM.
7. Copy the DKIM public key from the /etc/dkim.pub file. You can do this using the following command:

   ````
   cat /etc/dkim.pub
   ````

   Select and copy the entire string of characters that is displayed.
8. Go back to the Linode dashboard and click on the "Add a Record" button to add the DKIM record for your domain.
   - In the "Add a Record" window, select "TXT" from the "Record Type" drop-down menu. In the "Hostname" field, enter "default._domainkey".
   - In the "Text Data" field, enter the following DKIM record:

   ````
   v=DKIM1; k=rsa; p=<your DKIM public key>
   ````

   Replace "<your DKIM public key>" with the DKIM public key that you copied in step 7.
9. Click on the "Save" button to add the DKIM record to your DNS zone.
10. Update the iRedMail configuration file to enable DKIM signing. Connect to your Linode server using SSH and open the iRedMail configuration file using the following command:

   ````
   sudo nano /opt/iredapd/settings.py
   ````

   Uncomment the following lines by removing the "#" symbol at the beginning of each line:

   ````
   DKIM_SIGN_ENABLE = True
   DKIM_SELECTOR = 'default'
   DKIM_PRIVATE_KEY = '/etc/dkim.key'
   ````

   Save the file and exit.
11. Restart the iRedMail service to apply the changes:

   ````
   sudo systemctl restart iredapd
   ````

Congratulations! You've successfully configured SPF and DKIM records for your Linode Ubuntu Server with iRedMail. This should help improve email deliverability and reduce the likelihood of your emails being marked as spam.
