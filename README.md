# Setup and secure a Web Server

## Objective

The objective of this project is to set up and secure a web server in a Linux environment, ensuring its availability, performance, and protection against potential threats. The lab will involve installing and configuring a web server (such as Apache or Nginx), securing it with SSL/TLS encryption for secure HTTP (HTTPS), configuring firewall rules to limit access, and implementing access controls through user authentication and file permissions. Additionally, security best practices such as setting up automatic security updates, configuring intrusion detection systems, and testing the server for vulnerabilities will be covered.

### Skills Learned

- Web Server Installation and Configuration
- SSL/TLS Configuration for HTTPS
- Firewall and Network Security
- User Authentication and Access Control
- Hardening the Web Server
- Monitoring and Logging
- Intrusion Detection and Prevention

### Tools Used

- ufw (Uncomplicated Firewall) A simpler frontend for iptables.
- Ubuntu Linux OS
- Apache/Nginx Logs (for monitoring web server activity and identifying issues or attacks).
- Terminal CLI

## Steps
Make sure your system is up to date
sudo apt-get update && sudo apt-get upgrade
![image](https://github.com/user-attachments/assets/b18414a3-df21-4577-9244-cf75392d99ff)
Install Apache which is gonna be the Web Server
sudo apt-get install apache2
Then setup a firewall and add some rules
To enable UFW (Uncomplicated Firewall) on Kali Linux, you'll need to follow these steps:
1. Open a terminal in Kali Linux.
2. Check if UFW is installed (if not, you'll need to install it):
![image](https://github.com/user-attachments/assets/3c33aef8-b373-42c7-b96e-d21a876e1cf7)

Enable UFW:
1. Check UFW status:
sudo ufw status
![image](https://github.com/user-attachments/assets/2706a803-5d5f-4223-8213-4b4ba60694fa)
This will show whether the firewall is active and list the currently allowed/denied rules.
2. Add firewall rules (if needed). For example, to allow SSH:
• sudo ufw allow ssh
• sudo ufw allow http
• sudo ufw allow https
• sudo ufw allow 8080/tcp
• sudo ufw allow 1000:2000/tcp
• sudo ufw allow ‘Apache’
• sudo ufw allow all
![image](https://github.com/user-attachments/assets/f8a8f9a6-415d-4521-b179-c86639f944bc)

3. To disable UFW (if necessary):
sudo ufw disable
![image](https://github.com/user-attachments/assets/67284e4a-3b27-492a-8811-d18ebb92a789)
Now your firewall is enabled and running on Kali Linux. You can customize the rules
according to your requirements.
Verify rules configured:
sudo ufw status
![image](https://github.com/user-attachments/assets/aa2fd23c-97f3-4192-8415-5723348b2d75)
sudo ufw status numbered
![image](https://github.com/user-attachments/assets/3847eaea-2876-4f32-aba2-45becbd812e0)
Finally we wanna do is secure the web server by installing SSL and TLS
Step 1: Install Certbot
Certbot is a tool that automates the process of obtaining and renewing SSL certificates from Let’s Encrypt.
1. Update your system:
sudo apt update
sudo apt upgrade
2. Install Certbot and the web server plugin (for Apache or Nginx):
o For Apache:
sudo apt install certbot python3-certbot-apache
![image](https://github.com/user-attachments/assets/a1bc8afb-2527-46a3-81b3-8d349d8da441)
For Nginx:
bash code
sudo apt install certbot python3-certbot-nginx

Step 2: Obtain an SSL Certificate
Now, you'll obtain an SSL certificate from Let’s Encrypt.

1. For Apache:
sudo certbot –apache
![image](https://github.com/user-attachments/assets/ae41ac54-5980-42cb-9e66-2ff89a1d5e2a)

For Nginx:
bash
Copy code
sudo certbot --nginx
Certbot will automatically configure your web server and obtain the certificate. You’ll be
prompted to enter your domain name(s) and choose whether to redirect HTTP traffic to HTTPS
(which is recommended).
Step 3: Verify the Certificate Installation
Once Certbot has successfully obtained and installed the SSL certificate, you should check your
server to verify it’s using HTTPS.
1. For Apache: Certbot automatically modifies the configuration files and restarts Apache,
so no further action should be needed.
2. For Nginx: Certbot will also modify the Nginx configuration, but it’s good practice to
check the configuration file:
sudo nano /etc/nginx/sites-available/default
Look for the following lines under the server block:
listen 443 ssl;
ssl_certificate /etc/letsencrypt/live/your_domain/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/your_domain/privkey.pem;
After confirming the configuration is correct, reload Nginx:
sudo systemctl reload nginx
Step 4: Automate Certificate Renewal
Let’s Encrypt certificates are valid for 90 days, but Certbot can automatically renew them. The
following command simulates a renewal to make sure it’s set up correctly:
sudo certbot renew --dry-run
![image](https://github.com/user-attachments/assets/75cab431-734a-4aab-8190-ced4e8a405ef)
Certbot installs a cron job by default to handle renewals automatically, but running the dry-run
command ensures the process is working.
Step 5: Enable TLS for Extra Security
To ensure your web server is using the latest TLS protocols (like TLS 1.2 and TLS 1.3), you can
modify your configuration.
1. For Apache: Open the SSL configuration file:
sudo nano /etc/apache2/mods-available/ssl.conf
![image](https://github.com/user-attachments/assets/4924c0c2-655a-4288-8d95-54a63db34c37)
Add or modify the following line to disable older SSL protocols and enable only TLS:
SSLProtocol all -SSLv2 -SSLv3 -TLSv1 -TLSv1.1 +TLSv1.2 +TLSv1.3
![image](https://github.com/user-attachments/assets/8272e91e-221d-45e7-9565-d3fcb7a1e5d5)
Then, restart Apache:
sudo systemctl restart apache2
![image](https://github.com/user-attachments/assets/50d5d9a4-7da7-4a43-bdd9-68bfbf2d95a4)
2. For Nginx: Open the configuration file:
bash code
sudo nano /etc/nginx/nginx.conf
Under the server block, add or modify:
nginx
Copy code
ssl_protocols TLSv1.2 TLSv1.3;
Reload Nginx:
bash
Copy code
sudo systemctl reload nginx

Step 6: Test Your SSL/TLS Configuration

You can verify your server’s SSL/TLS configuration using SSL Labs' SSL Test.
That's it! Your web server should now be secured with SSL and TLS.

# That’s it we have a fully running secure web server!








