# iRedMail: A Comprehensive Overview

## 1. Why iRedMail?

iRedMail is a popular, open-source mail server solution that provides a fully functional mail system for enterprises and individuals. It simplifies the installation and configuration of a mail server by bundling several open-source components such as Postfix, Dovecot, SpamAssassin, and ClamAV. With iRedMail, users can have a reliable, secure, and customizable mail server up and running quickly.

### Key Features of iRedMail:
- **Open-source and free**: iRedMail offers a completely free solution, though a commercial version, iRedMail Pro, is available for added features and support.
- **Supports multiple operating systems**: It can be installed on a variety of Linux and BSD distributions, including Ubuntu, CentOS, Debian, OpenBSD, and FreeBSD.
- **Web-based admin panel**: It includes a web-based management interface for user and domain administration.
- **Security-focused**: iRedMail comes pre-configured with firewalls and additional security layers like Fail2ban and Let's Encrypt support for SSL certificates.
  
### Pros and Cons of iRedMail

| **Pros** | **Cons** |
|----------|----------|
| **Open-source and free**: No cost for basic usage, making it a cost-effective option. | **Requires technical knowledge**: Initial setup and maintenance require familiarity with Linux servers and email protocols. |
| **Pre-configured components**: Includes Postfix, Dovecot, SpamAssassin, and ClamAV for a complete mail server solution. | **Limited to server environments**: Requires a dedicated server or VPS, which may not be ideal for all users. |
| **Easy installation**: Streamlined setup process compared to manual mail server installation. | **Web-based admin limited in free version**: The free version lacks advanced management features available in the Pro edition. |
| **Supports multiple email domains**: Manage multiple email domains from a single server instance. | **Resource-intensive**: Can be resource-heavy on smaller servers, especially when handling high traffic. |
| **SSL and security features**: Integrates with Let's Encrypt for automatic SSL certificates and uses Fail2ban to protect against brute-force attacks. | **Limited official support**: The open-source version relies heavily on community support, and users may need to troubleshoot issues independently. |
| **Customizable**: Allows for customization of mail server configurations to fit individual or enterprise needs. | **Complex upgrades**: Upgrading between major versions can sometimes require careful planning and testing. |
| **Supports multiple authentication methods**: LDAP, MySQL, and more, making it flexible for different setups. | |

In summary, iRedMail is an excellent solution for those seeking a powerful, open-source mail server with a focus on security and ease of use. However, it is best suited for users with some technical experience who are comfortable with server administration.

## 2. Prerequisites for iRedMail Installation

Before installing iRedMail, there are several prerequisites that need to be met to ensure a smooth installation and optimal performance. The following table outlines the necessary requirements for hardware, software, and system configurations.

### Prerequisites Table

| **Category**      | **Requirement**                                                                                 | **Details**                                                                                 |
|-------------------|-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| **Operating System** | Supported Linux or BSD distributions: Ubuntu, CentOS, Debian, Rocky Linux, FreeBSD, OpenBSD | Ensure your system runs a compatible version of one of the supported distributions.          |
| **RAM**           | Minimum 1 GB (2 GB+ recommended for larger setups)                                               | iRedMail can run on low memory, but more RAM is recommended for handling higher email traffic. |
| **Disk Space**     | Minimum 20 GB (depends on email storage needs)                                                  | The amount of disk space needed depends on the expected volume of emails and attachments.     |
| **Processor**      | 64-bit CPU                                                                                      | A modern 64-bit processor is required for optimal performance and compatibility.             |
| **Root Access**    | Root or sudo access to the server                                                              | Administrative privileges are needed for installing and configuring iRedMail components.     |
| **Domain Name**    | Fully Qualified Domain Name (FQDN)                                                              | Required for mail server configuration and SSL certificate setup.                            |
| **DNS Records**    | Valid MX and A records                                                                          | Ensure the domain's DNS is correctly configured with MX records pointing to the server.      |
| **Open Ports**     | Ports 25, 80, 443, 587, 993                                                                     | These ports must be open for SMTP, HTTP, HTTPS, and IMAP communications.                     |
| **Email Protocols**| IMAP/SMTP support                                                                               | iRedMail uses IMAP (Dovecot) and SMTP (Postfix) for handling email communications.           |
| **Software Packages** | Basic server software (Postfix, Dovecot, MySQL/MariaDB, Apache/Nginx)                           | These are automatically installed by iRedMail but require compatibility on your OS.          |
| **SSL/TLS**        | Optional: Let's Encrypt or any SSL provider                                                     | iRedMail can automatically configure SSL certificates via Let's Encrypt for secure connections. |

### Additional Considerations:
- **Backups**: It's recommended to have a backup strategy in place for the server and email data.
- **VPS/Cloud Hosting**: Ensure your cloud or VPS provider allows email traffic and doesn’t block necessary ports, as some providers restrict email hosting services.

## 3. Big Picture View: iRedMail Components and Functionality

iRedMail integrates various open-source components to build a robust and secure email system. Each component serves a specific function to ensure email delivery, management, and security. Here's an overview of the main components and their roles in an iRedMail setup:

### Components of iRedMail

| **Component**         | **Functionality**                                                                                                    |
|-----------------------|----------------------------------------------------------------------------------------------------------------------|
| **Postfix**           | Postfix is the core mail transfer agent (MTA) used for sending and receiving emails. It handles SMTP (Simple Mail Transfer Protocol) communication, ensuring that emails are properly routed between servers. |
| **Dovecot**           | Dovecot is the IMAP and POP3 server used for retrieving and storing emails. It ensures that users can access their emails from various clients, securely handling mail retrieval. |
| **Amavisd**           | Amavisd integrates with Postfix to scan incoming and outgoing emails for spam and viruses. It acts as a gateway, ensuring that emails are filtered and safe. |
| **SpamAssassin**      | SpamAssassin is a spam filtering tool used to detect and filter spam emails. It uses a combination of rules and machine learning to flag suspicious messages and prevent inbox clutter. |
| **ClamAV**            | ClamAV is an open-source antivirus engine that scans emails for malicious attachments and viruses. It ensures that email attachments are safe to download and view. |
| **OpenLDAP or MySQL/MariaDB** | iRedMail supports both OpenLDAP and MySQL/MariaDB for managing users and domains. These databases store email user information, including credentials and mailbox details. |
| **Fail2Ban**          | Fail2Ban provides additional security by monitoring server logs for suspicious login attempts. If a brute-force attack is detected, Fail2Ban can block the attacker’s IP address. |
| **iRedAdmin (Pro)**   | iRedAdmin is the web-based administration panel used for managing domains, users, and server configurations. The Pro version provides enhanced features for larger organizations. |
| **SOGo/ Roundcube**   | These are webmail clients that allow users to access their email through a web browser. They provide an intuitive interface for sending and receiving emails, managing contacts, and more. |
| **Let's Encrypt**     | Let's Encrypt is an SSL certificate authority that provides free SSL certificates. iRedMail integrates with Let's Encrypt to automatically configure and renew certificates for secure email communication. |

### Functional Flow of iRedMail

1. **Receiving an Email (SMTP via Postfix)**:
   - When an email is sent to an iRedMail server, **Postfix** receives it via SMTP.
   - Postfix checks the email’s destination and routes it appropriately.

2. **Spam and Virus Filtering (Amavisd, SpamAssassin, ClamAV)**:
   - The email is passed through **Amavisd**, which uses **SpamAssassin** to check for spam and **ClamAV** to scan for viruses.
   - If the email passes these checks, it proceeds to the next stage.

3. **Storing the Email (Dovecot)**:
   - The email is then stored in the user's mailbox, managed by **Dovecot**.
   - Users can access their emails using an IMAP or POP3 client, such as a webmail interface or a desktop email client.

4. **User Authentication (LDAP/MySQL/MariaDB)**:
   - Users authenticate through **OpenLDAP** or **MySQL/MariaDB**. These databases store user credentials and mailbox settings.

5. **Webmail Access (SOGo/Roundcube)**:
   - Users can log into the webmail client, either **SOGo** or **Roundcube**, to access their emails, manage contacts, and more.

6. **Security (Fail2Ban, SSL)**:
   - **Fail2Ban** monitors for suspicious activity, such as repeated failed login attempts, and blocks offending IP addresses.
   - **Let's Encrypt** ensures that the server's SSL certificates are up to date, providing encryption for both webmail and mail clients.

### Visual Summary

Here’s a visual representation of the iRedMail architecture, showing how its components interact to deliver, filter, and store emails, as well as provide security and authentication:

- [Link to official iRedMail documentation images](https://docs.iredmail.org/used.components.html)

Alternatively, you can use this custom visualization:

![iRedMail Functional Overview](./images/1.webp) 

This architecture ensures a robust, secure, and manageable mail server that can handle the needs of businesses and individuals alike.


## 4. Ports Used by iRedMail: Public and Private Configuration

iRedMail uses a variety of ports for different services, including email communication, webmail access, and administrative purposes. Some of these ports need to be accessible publicly, while others should remain private for security reasons.

### Table: iRedMail Ports Overview

| **Port Number** | **Protocol** | **Service**       | **Publicly Accessible** | **Details**                                                                 |
|-----------------|--------------|-------------------|-------------------------|-----------------------------------------------------------------------------|
| **25**          | SMTP         | Postfix (MTA)     | Yes                     | Used for sending and receiving emails between mail servers. Must be open to ensure email delivery. |
| **465**         | SMTPS        | Postfix (MTA)     | Yes                     | SMTPS (SMTP over SSL/TLS) is used for securely sending email messages. Needs to be open for secure email transmission from mail clients. |
| **587**         | SMTP (STARTTLS)| Postfix (MTA)   | Yes                     | Submission port for mail clients to send emails using SMTP with STARTTLS encryption. Typically used by mail clients such as Thunderbird or Outlook. |
| **110**         | POP3         | Dovecot (POP3)    | Optional (but not recommended) | Used for retrieving emails via POP3 protocol. Typically closed in modern setups, as IMAP (port 143/993) is preferred. |
| **143**         | IMAP         | Dovecot (IMAP)    | Yes                     | Used for retrieving emails via IMAP protocol (unencrypted). Should be used with STARTTLS on this port or prefer IMAPS (port 993). |
| **993**         | IMAPS        | Dovecot (IMAP)    | Yes                     | Secure IMAP over SSL/TLS. Should be open to allow users to securely retrieve emails. |
| **80**          | HTTP         | Nginx/Apache      | Yes                     | Required for initial Let's Encrypt SSL certificate issuance. Should be redirected to HTTPS (port 443). |
| **443**         | HTTPS        | Nginx/Apache      | Yes                     | Secure webmail and admin interface access via HTTPS. Used by clients to access webmail (Roundcube/SOGo) and the iRedAdmin panel. |
| **8080**        | HTTP (optional)| Web admin panels | No                      | Occasionally used for internal administrative access, but generally not needed unless configured that way. Best kept closed or limited to internal network. |
| **3306**        | MySQL/MariaDB | Database          | No                      | MySQL/MariaDB database server. Only required for internal communication between services, not publicly accessible. Ensure this port is closed externally. |
| **389**         | LDAP         | OpenLDAP          | No                      | LDAP server used for user authentication. Should remain internal and not publicly accessible. Secure LDAP uses port 636. |
| **636**         | LDAPS        | OpenLDAP (Secure) | No                      | Secure LDAP (over SSL). Should only be accessible within internal network or via secure VPN. |
| **53**          | DNS          | DNS Server        | No (unless hosting DNS) | Required if you are hosting a DNS server, but otherwise this should not be publicly accessible. Use external DNS services instead. |
| **995**         | POP3S        | Dovecot (Secure POP3) | Yes, if POP3 used     | Secure POP3 protocol for receiving mail over SSL. Should be used only if POP3 is required. |

### Detailed Information:

- **Publicly Accessible Ports**:
  - **SMTP (Port 25, 465, 587)**: These ports are essential for email transfer between mail servers and clients. SMTP port 25 is required for communication between mail servers, while port 465 (SMTPS) and port 587 (Submission with STARTTLS) are necessary for secure email sending from clients.
  - **IMAP (Port 143, 993)**: For users accessing their emails via IMAP, the secure version (IMAPS, port 993) should be open, with port 143 used for IMAP connections that are later upgraded via STARTTLS.
  - **Web Access (Port 80, 443)**: Port 443 is needed for secure HTTPS connections to webmail (SOGo/Roundcube) and the iRedMail admin panel. Port 80 should be open temporarily for Let's Encrypt certificate verification, but traffic should be redirected to HTTPS for security.

- **Ports That Should Not Be Publicly Accessible**:
  - **Database (Port 3306)**: The MySQL/MariaDB database port should be closed to external traffic, as it is only used internally by iRedMail components.
  - **LDAP (Ports 389, 636)**: LDAP and Secure LDAP should be kept within the internal network to avoid exposing sensitive user authentication data. These ports should be closed externally.
  - **POP3 (Port 110, 995)**: POP3 is generally considered outdated and less secure than IMAP. If used, POP3 should be over SSL (port 995), though IMAP is recommended for modern setups.
  
### Security Best Practices:

- **Use SSL/TLS for All Services**: Always prefer secure protocols (IMAPS, SMTPS, LDAPS) over their unencrypted counterparts to protect data in transit.
- **Firewall Configuration**: Ensure that only the necessary ports are open to the public, with all others closed or restricted to the internal network or through a VPN.
- **Port 25 Handling**: While port 25 is required for email delivery, some cloud providers block this port to prevent abuse. Be sure to check with your hosting provider and request an unblock if needed.


