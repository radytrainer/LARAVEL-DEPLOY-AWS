## CREATE VIRTUAL HOST IN CENTOS

### STEP 1: CONFIGURE VIRTUAL HOST 
We have to create two directories called <code>sites-available</code> and <code>sites-enabled</code>. Apache store website in sites-available folder and Apache server website from sites-enabled folder. Let’s make these folders:

```
sudo mkdir /etc/httpd/sites-available /etc/httpd/sites-enabled
```
Open <code>httpd.conf</code> file using this command:

```
sudo vi /etc/httpd/conf/httpd.conf
```
Add this line to the end of the file:
```
IncludeOptional sites-enabled/*.conf
```
After that let’s make a configuration file in the sites-available directory:
```
sudo vi /etc/httpd/sites-available/example.com.conf
```
### Now paste this configuration block:
```
<VirtualHost *:80>
   ServerName example.com
   ServerAlias www.example.com
   DocumentRoot /var/www/html/project-name/front/dist
</VirtualHost>

<VirtualHost *:3000>
    ServerName example.com
   ServerAlias www.example.com
   DocumentRoot /var/www/html/project-name/back/public
</VirtualHost>

```
> You maybe need some log file but here just basic

We have to create a symbolic link for each virtual host in the sites-enableddirectory:

```
sudo ln -s /etc/httpd/sites-available/example.com.conf /etc/httpd/sites-enabled/example.com.conf
```
Our virtual host is configured and ready to serve.

### STEP 2: Adjust SELinux Permissions

It’s recommended to adjust SELinux permissions. Run this command to set universal permissions:

```
sudo setsebool -P httpd_unified 1
```

### STEP 3: Test the Virtual Host

Restart the web server and visit the website to see the welcome message.

```
sudo systemctl restart httpd
```