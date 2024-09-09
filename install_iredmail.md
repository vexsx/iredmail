# How to Easily Set Up a Full-Featured Mail Server on Ubuntu 22.04 with iRedMail

## Step 1: Choose the Right Hosting Provider and Buy a Domain Name
To set up a full-featured email server with iRedMail, you need a server with at least **4GB** RAM & **4 core** CPU, because after the installation, your server will use more than 2GB of RAM.

**It is highly recommended that you install iRedMail on a clean fresh Ubuntu 22.04 server. If you use an existing production server, iRedMail might wipe away your existing database.**



## Step 2: Creating DNS MX Record
The MX record specifies which host or hosts handle emails for a particular domain name. For example, the host that handles emails for milad.com is `mail.milad.com`. If someone with a Gmail account sends an email to [`[email protected]`](https://www.linuxbabe.com/cdn-cgi/l/email-protection), then Gmail server will query the MX record of milad.com. When it finds out that `mail.milad.com` is responsible for accepting email, it then query the A record of `mail.milad.com` to get the IP address, thus the email can be delivered.

In your DNS manager, create a MX record for your domain name. Enter `@` in the Name field to represent the main domain name, then enter `mail.your-domain.com` in the Value field.

![iRedMail MX](./images/iredmail-email-server-create-MX-record.png) 

**Note:** The hostname for MX record can not be an alias to another name. Also, It’s highly recommended that you use hostnames, rather than bare IP addresses for MX record.

Your DNS manager may require you to enter a preference value (aka priority value). It can be any number between 0 and 65,356. A small number has higher priority than a big number. It’s recommended that you set the value to 0, so this mail server will have the highest priority for receiving emails. After creating MX record, you also need to create an A record for `mail.your-domain.com` , so that it can be resolved to an IP address. If your server uses IPv6 address, be sure to add AAAA record.

A record : 

![iRedMail A rec](./images/A-record.png) 

Hint: If you use Cloudflare DNS service, you should not enable the CDN feature when creating A record for `mail.your-domain.com`. Cloudflare does not support SMTP proxy.

## Step 3: Configuring Hostname

Log into your server via SSH then run the following command to update existing software packages.
```
sudo apt update && apt upgrade && apt install -y gzip dialog
```
I strongly recommend creating a sudo user for managing your server rather than using the default root user. Run the following command to create a user. Replace username with your preferred username.

```
adduser username
```

![iRedMail user](./images/adduser-scalahosting.png) 

Then add the user to the `sudo` group.

Then switch to the new user.
```
su - username
```

Next, set a fully qualified domain name (FQDN) for your server with the following command.

```
sudo hostnamectl set-hostname mail.your-domain.com
```
We also need to update /etc/hosts file with a command line text editor like Nano.
```
sudo nano /etc/hosts
```
Edit it like below. (Use arrow keys to move the cursor in the file.)
```
127.0.0.1       mail.your-domain.com mail localhost
```
Save and close the file. (To save a file in Nano text editor, press `Ctrl+O`, then press `Enter` to confirm. To close the file, press `Ctrl+X`.)

To see the changes, re-login and then run the following command to see your hostname.

```
hostname -f
```

## Step 4: Setting up Mail Server on Ubuntu 22.04 with iRedMail

Run the following commands to download the latest version of iRedMail script installer from its Github repository.

```
wget https://github.com/iredmail/iRedMail/archive/refs/tags/1.7.1.tar.gz
```
Extract the archived file.
```
tar xvf 1.7.1.tar.gz
```
Then cd into the newly-created directory.
```
cd iRedMail-1.7.1/
```
Add executable permission to the iRedMail.sh script. then run.
```
chmod +x iRedMail.sh
sudo bash iRedMail.sh
```
The mail server setup wizard will appear. Use the Tab key to select Yes and press Enter.

![iRedMail f1](./images/ubuntu-18.04-iredmail-server.png) 

The next screen will ask you to select the mail storage path. You can use the default one /var/vmail, so simply press Enter.  

![iRedMail f2](./images/iredmail-1.0-default-storage-path.png) 


Then choose whether you want to run a web server. It’s highly recommended that you choose to run a web server because you need the web-based admin panel to add email accounts. Also, it allows you to access the Roundcube webmail. By default, Nginx web server is selected, so you can simply press Enter. (An asterisk indicates the item is selected.)


![iRedMail f3](./images/iredmail-1.0-nginx-web-server.png) 


Then select the storage backend for email accounts. Choose one that you are familiar with. This tutorial chose MariaDB. Press up and down arrow key and press the space bar to select.


![iRedMail f4](./images/ubuntu-18.04-email-server-1.png)   

If you selected MariaDB or MySQL, then you will need to set the MySQL root password.


![iRedMail f5](./images/6ubuntu-18.04-mail-server-1.png)   

Note that if you selected MariaDB, then you don’t need password to log into MariaDB shell. Instead of running the normal command mysql -u root -p, you can run the following command to login, with sudo and without providing MariaDB root password.

```
sudo mysql -u root
```

This is because the MariaDB package on Ubuntu 22.04 uses unix_socket authentication plugin, which allows users to use OS credentials to connect to MariaDB, but you still need to set root password in iRedMail setup wizard.

Next, enter your first mail domain. You can add additional mail domains later in the web-based admin panel. This tutorial assumes that you want an email account like. In that case, you need to enter **your-domain.com** here, without sub-domain. Do not press the space bar after your domain name. I think iRedMail will copy the space character along with your domain name, which can result in installation failure.

![iRedMail f6](./images/set-up-mail-server-on-ubuntu-18.04-1.png)   

Next, set a password for the mail domain administrator.  

![iRedMail f7](./images/7ubuntu-18.04-email-server-step-by-step-1.png)   

Choose optional components. By default, 4 items are selected. Please note that SOGo groupware doesn’t support Ubuntu 22.04 right now, so don’t select it.

![iRedMail f8](./images/8iredmail-components.png)  

Now you can review your configurations. Type Y to begin the installation of all mail server components.

![iRedMail f9](./images/9iredmail-review-1.png)  

it is depending on you to enable firewall rules provided by iRedMail , becuase of outside firewall like (security groups and ...) but its better to enable it 

![iRedMail f10](./images/10iredmail-firewall-rules-fail2ban-1.png)  

Now iRedMail installation is complete. You will be notified the URL of webmail, SOGo groupware and web admin panel and the login credentials. The iRedMail.tips file contains important information about your iRedMail server.

![iRedMail f11](./images/11iredmail-full-featured-mail-server-1.png)  

Reboot your Ubuntu 22.04 server.

```
sudo reboot now
```

Once your server is back online, you can visit the web admin panel.

```
https://mail.your-domain.com/iredadmin/
```
*Note* that in the above URL, the sub-directory for accessing the admin panel is /iredadmin/, not /iredmail/. And because it’s using a self-signed TLS certificate, you need to add security exception in your web browser.


## Step 5: Installing Let’s Encrypt TLS Certificate 

Since the mail server is using a self-signed TLS certificate, both desktop mail client users and webmail client users will see a warning. To fix this, we can obtain and install a free Let’s Encrypt TLS certificate.

### Obtaining the Certificate  

First, log into your server again via SSH and run the following commands to install Let’s Encrypt (certbot) client on Ubuntu 22.04.

```
sudo apt install certbot python3-certbot-nginx -y
```

iRedMail has already configured TLS settings in the default Nginx virtual host, so here I recommend using the webroot plugin, instead of nginx plugin, to obtain certificate. Run the following command. Replace red text with your actual data.

```
sudo certbot certonly --webroot --agree-tos --email[email protected] -d mail.your-domain.com -w /var/www/html/
```
When it asks you if you want to receive communications from EFF, you can choose No.

![iRedMail f12](./images/12iredmail-letsencrypt.png)

If everything went well, you will see the following text indicating that you have successfully obtained a TLS certificate. Your certificate and chain have been saved at /etc/letsencrypt/live/mail.your-domain.com/ directory.

![iRedMail f13](./images/13iredmail-certbot.png)

### Failure to Obtain TLS Certificate


If certbot failed to obtain TLS certificate, maybe it’s because your DNS records are not propagated to the Internet. Depending on the domain registrar you use, your DNS record might be propagated instantly, or it might take up to 24 hours to propagate. You can go to [https://dnsmap.io](https://dnsmap.io/), enter your mail server’s hostname (`mail.your-domain.com`) to check DNS propagation.

If certbot failed to obtain a certificate and you saw the following message,
```
Failed authorization procedure. mail.milad.com (http-01): urn:ietf:params:acme:error:connection :: The server could not connect to the client to verify the domain :: Fetching https://mail.milad.com/.well-known/acme-challenge/IZ7hMmRE4ZlGW7cXYoq2Lc_VrFzVFyfW6E0pzNlhiOA: Timeout during connect (likely firewall problem)
```

It might be that you have set AAAA record for mail.your-domain.com, but Nginx web server doesn’t listen on IPv6 address. To fix this error, edit the /etc/nginx/sites-enabled/00-default.conf file

```
sudo nano /etc/nginx/sites-enabled/00-default.conf
```
Find the following line.
```
#listen [::]:80;
```
Remove the # character to enable IPv6 for this Nginx virtual host.
```
listen [::]:80;
```

Save and close the file. Then edit the SSL virtual host `/etc/nginx/sites-enabled/00-default-ssl.conf`.

```
sudo nano /etc/nginx/sites-enabled/00-default-ssl.conf
```
Add the following line.
```
listen [::]:443 ssl http2;
```

![iRedMail f14](./images/14iredmail-certbot-renew.png)

Save and close the file. Then test Nginx configuration.
```
sudo nginx -t
```
If the test is successful, reload Nginx for the change to take effect.

```
sudo systemctl reload nginx
```
Run the following command again to obtain TLS certificate. Replace red text with your actual data.
```
sudo certbot certonly --webroot --agree-tos --email[email protected] -d mail.your-domain.com -w /var/www/html/
```

Now you should be able to successfully obtain TLS certificate.

### Installing the Certificate in Nginx

After obtaining a TLS certificate, let’s configure Nginx web server to use it. Edit the SSL template file.

```
sudo nano /etc/nginx/templates/ssl.tmpl
```
Find the following 2 lines.
```
ssl_certificate /etc/ssl/certs/iRedMail.crt;
ssl_certificate_key /etc/ssl/private/iRedMail.key;
```
Replace them with:
```
ssl_certificate /etc/letsencrypt/live/mail.your-domain.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/mail.your-domain.com/privkey.pem;
```

Save and close the file. Then test nginx configuration and reload.

```
sudo nginx -t

sudo systemctl reload nginx

```

Visit iRedMail admin panel again, your web browser won’t warn you any more because Nginx is now using a valid TLS certificate.

![iRedMail f15](./images/15iredadmin.png)

### Installing TLS Certificate in Postfix and Dovecot   

We also need to configure Postfix SMTP server and Dovecot IMAP server to use the Let’s Encrypt issued certificate so that desktop mail client won’t display security warning. Edit the main configuration file of Postfix.

```
sudo nano /etc/postfix/main.cf
```

Find the following 3 lines. (line 95, 96, 97).

```
smtpd_tls_key_file = /etc/ssl/private/iRedMail.key
smtpd_tls_cert_file = /etc/ssl/certs/iRedMail.crt
smtpd_tls_CAfile = /etc/ssl/certs/iRedMail.crt
```
Replace them with:

```
smtpd_tls_key_file = /etc/letsencrypt/live/mail.your-domain.com/privkey.pem
smtpd_tls_cert_file = /etc/letsencrypt/live/mail.your-domain.com/cert.pem
smtpd_tls_CAfile = /etc/letsencrypt/live/mail.your-domain.com/chain.pem
```
Save and close the file. Then reload Postfix.

```
sudo systemctl reload postfix
```

Next, edit the main configuration file of Dovecot.

```
sudo nano /etc/dovecot/dovecot.conf
```

Fine the following 2 lines. (line 47, 48)

```
ssl_cert = </etc/ssl/certs/iRedMail.crt
ssl_key = </etc/ssl/private/iRedMail.key
```

Replace them with:

```
ssl_cert = </etc/letsencrypt/live/mail.your-domain.com/fullchain.pem
ssl_key = </etc/letsencrypt/live/mail.your-domain.com/privkey.pem
```

Save and close the file. Then reload dovecot.

```
sudo systemctl reload dovecot
```
From now on, desktop mail users won’t see security warnings.

## Step 6: Sending Test Email

Log into iredadmin panel with the postmaster mail account ([email protected]). In the `Add` tab, you can add additional domains or email addresses.

![iRedMail f16](./images/16add-email-addresses-in-iredadmin.png)

After you create a user, you can visit the Roundcube webmail address and login with the new mail user account.

```
https://mail.your-domain.com/mail/
```

![iRedMail f17](./images/17iredmail-roundcube-webmail.png)

Now you can test email sending and receiving. Please note that you may need to wait a few minutes to receive emails because iRedMail by default enables greylisting, which is a way to tell other sending SMTP servers to try again in a few minutes. The following line in mail log file `/var/log/mail.log` indicates greylisting is enabled.

```
Recipient address rejected: Intentional policy rejection, please try again later;
```
### Adding Swap Space

ClamAV is used to scan viruses in email messages. ClamAV can use a fair amount of RAM. If there’s not enough RAM on your server, ClamAV won’t work properly, which will prevent your mail server from sending emails. You can add a swap file to your server to increase the total RAM on your server. (Note that using swap space on the server will degrade server performance. If you want better performance, you should upgrade the physical RAM instead of using swap space.)

To add swap space on the server, first, use the `fallocate` command to create a file. For example, create a file named swapfile with 1G capacity in root file system:


```
sudo fallocate -l 1G /swapfile
```

Then make sure only root can read and write to it.

```
sudo chmod 600 /swapfile
```

Format it to swap:
```
sudo mkswap /swapfile
```

Output:
```
Setting up swapspace version 1, size = 1024 MiB (1073737728 bytes)
no label, UUID=0aab5886-4dfb-40d4-920d-fb1115c67433
``` 
