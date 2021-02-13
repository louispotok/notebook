I recently set up Matomo to track visits to recoolit.com. Here's how I did it.

The setup:
1. Recoolit.com is hosted by Webflow
2. The domain is managed with Google Domains
3. I am running the Matomo self-hosted version on a digital ocean droplet.

The [official docs](https://matomo.org/docs/installation/) are okay but contain a massive gap. Where it says "Open your web browser and navigate to the URL to which you uploaded Matomo" -- this was not at all trivial for me. I used these two tutorials ([1](https://www.linuxbabe.com/ubuntu/install-matomo-web-analytics-piwik-ubuntu-18-04-apache-nginx) and [2](https://artofadventuring.com/install-matomo-piwik-digitalocean/)) heavily. Also, I probably forgot some steps. So if something is missing, compare to these tutorials and fill in the steps.

# Configure your subdomain
As far as I understand, you need your matomo server running on a named domain rather than just your server IP address to do this, because 

1. The Matomo tracking call uses https
2. To get a certificate for https, you need a named domain.

This could be wrong. But I ended up using a subdomain `analytics.recoolit.com`. To do this, I went to my DNS settings in Google Domains, and added a new `A` record from `analytics` -> `ip address`. It takes a little while to propagate, so do this first.

# Install matomo
Ssh into your server and run:

```
# Download matomo
wget https://builds.matomo.org/matomo-latest.zip

# unzip it to the directory where the web server can run it
sudo apt install unzip
sudo unzip matomo-latest.zip -d /var/www/html
sudo chown -R www-data:www-data /var/www/html/matomo


# Install some other needed packages
sudo apt install apache2
sudo apt-get install php php-curl php-gd php-cli mysql-server php-mysql php-xml php-mbstring
```

# Prepare mysql
Matomo needs to store its data somewhere.
```bash
mysql -u root -p
```
And now run the setup. You'll need this username and password when running the wizard.
```SQL
CREATE DATABASE matomo;
# replace the username and password, leave the quotes, you can leave localhost
CREATE USER 'matomouser'@'localhost' IDENTIFIED BY 'strong-password-goes-here';
# replace the username, and localhost if you changed it before.
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES ON matomo.* TO 'matomouser'@'localhost';
```

# Configure Apache
```bash
# create  a new configuration file for this web server
vi /etc/apache2/sites-available/matomo.conf
```

Add the following, replacing your email address and ServerName.
```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName analytics.example.com
    DocumentRoot /var/www/html/matomo
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Note: I make the DocumentRoot `/matomo`. The analytics dashboard will have a login screen that is publicly available, so this whole web server is open. Without the `/matomo`, you end up making an Apache file navigator visible to the public, which I didn't want. There's probably a smarter way to do this, though.

Now, enable that configuration, disable the default, and restart the Apache service:
```bash
sudo a2ensite matomo.conf
sudo a2dissite 000-default.conf
sudo service apache2 start
```

# Enable HTTPS

I used the EFF's Certbot, following the installation instructions [here](https://certbot.eff.org/instructions).

This wil create a new configuration file `/etc/apache2/sites-available/matomo-le-ssl.conf`. When I went through this, I had a problem where it had copied the servername and document root from `matomo.conf` before I corrected those. So keep an eye out.

# Set up Matomo
Finally, we can go back to the official instructions and use the wizard. Go to `analytics.yourdomain.com` in your browser, which should open up a pretty-looking website. Follow the steps!

I had to enable two extensions, `mbstring` and `mysqli` by opening `/etc/php/7.4/cli/php.ini` and uncommenting lines 924 and 926. Semicolons comment out the line.
```
;extension=gmp
;extension=intl
;extension=imap
;extension=ldap
extension=mbstring
;extension=exif      ; Must be after mbstring as it depends on it
extension=mysqli
;extension=oci8_12c  ; Use with Oracle Database 12c Instant Client
;extension=odbc
;extension=openssl
;extension=pdo_firebird
```

I then had to `reboot` my server.

Note that there are two places you'll be entering a username and password. The first is for Mysql, it's the one you created earlier. The second is your login to the web interface (they call this "superuser", I think).

When this is done, you chould be able to go to `analytics.subdomain.com` and log into a dashboard.

Finally, copy the tracking code into Webflow's Custom Code tab, making sure that the URL it's trying to pull the script from is correct. I had to adjust this line.
```javascript
    var u="//analytics.subdomain.com/";
```
Fire up your site with dev tools open, make sure the matomo.js call resolves, and then go see your first pageview in your dashboard!