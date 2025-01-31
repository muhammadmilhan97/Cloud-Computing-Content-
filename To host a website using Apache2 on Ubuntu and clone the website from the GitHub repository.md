**To host a website using Apache2 on Ubuntu and clone the website from the GitHub repository**

### <a name="_hhg8t86at6b0"></a>**Step 1: Install Apache2**
If Apache2 isn't already installed, you can install it using:

sudo apt update

sudo apt install apache2

### <a name="_kkfs4ay7lfdp"></a>**Step 2: Clone the GitHub Repository**
Navigate to your desired directory where you want to host the website (typically under /var/www/html/ for Apache on Ubuntu). Clone the repository:

cd /var/www/html/

sudo git clone https://github.com/NasirNauman/webhtml

This will create a webhtml directory containing your website files.
### <a name="_g7mrijog7azr"></a>**Step 3: Configure Apache Virtual Host** 
If you need a specific configuration for your website, you can create a virtual host configuration file. For example, create a file named gci.conf in the Apache configuration directory:

sudo nano /etc/apache2/sites-available/gci.conf

Add the following configuration (adjust as per your needs):

<VirtualHost \*:80>

`    `ServerAdmin webmaster@example.com

`    `DocumentRoot /var/www/html/webhtml

`    `ServerName your\_domain.com

`    `ServerAlias www.your\_domain.com

`    `<Directory /var/www/html/webhtml>

`        `Options Indexes FollowSymLinks MultiViews

`        `AllowOverride All

`        `Require all granted

`    `</Directory>

`    `ErrorLog ${APACHE\_LOG\_DIR}/error.log

`    `CustomLog ${APACHE\_LOG\_DIR}/access.log combined

</VirtualHost>

Save and close the file (Ctrl+X, then Y, and Enter).
### <a name="_2kch3qyiwdyl"></a>**Step 4: Set the Default Document**
By default, Apache serves index.html or index.php when you access a directory. Since your main file is named Untitled-1.html, you need to configure Apache to use it as the default.

Edit the Apache configuration file:
bash
CopyEdit
sudo nano /etc/apache2/sites-available/gci.conf

Add or modify the DirectoryIndex directive to specify Untitled-1.html:
apache
CopyEdit
<Directory /var/www/html/webhtml>

`    `Options Indexes FollowSymLinks MultiViews

`    `AllowOverride All

`    `Require all granted

`    `DirectoryIndex Untitled-1.html

</Directory>

Save and exit the file (Ctrl+X, then Y, and Enter).



### <a name="_vxf19uqyq9vo"></a>**Step 5: Open the /etc/hosts File**
1. Open a terminal on your Ubuntu system.

Run the following command to edit the /etc/hosts file:

` `sudo nano /etc/hosts

### <a name="_7ii7zlmy0y80"></a>**Step 6: Add the Domain Mapping**
1. Scroll to the bottom of the file.

Add the following line, replacing your\_domain.com with the domain name you configured in your Apache configuration (e.g., gci.local):

` `127.0.0.1   your\_domain.com

If you want to include a www subdomain, you can also add it:

` `127.0.0.1   your\_domain.com [www.your_domain.com](http://www.your_domain.com)

### <a name="_oobfgtf2o83g"></a>**Step 7: Save and Exit**
1. After adding the line, press Ctrl+X to exit.
1. Press Y to confirm saving changes.
1. Press Enter to save the file.
### <a name="_db0mntdvvgj9"></a>**Step 8: Test Your Configuration**
1. Open your browser.
1. Navigate to http://your\_domain.com (or http://www.your\_domain.com if you included it).
1. It should resolve to the website hosted on localhost.

###
### <a name="_p14i7i2wh54"></a><a name="_4hgny1bvq3kt"></a>**Step 9: Enable the Virtual Host**
Enable the virtual host configuration you created:

sudo a2ensite gci.conf

### <a name="_g6lecck3f9bc"></a>**Step 10: Restart Apache**
To apply the changes, restart Apache:

sudo systemctl restart apache2

### <a name="_54pgd9qr5s8e"></a>**Step 11: Access Your Website**
You should now be able to access your website through your server's IP address or domain name (if configured). Open a web browser and enter http://your\_server\_ip or http://your\_domain.com to see your website.

