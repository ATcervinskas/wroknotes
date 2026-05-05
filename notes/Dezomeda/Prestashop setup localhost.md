
First of all check prestashop version is compatible with VM's php version

defines.inc.php - set _PS_MODE_DEV_ = true // enables log messages

app/config/parameters.php  - change db conncetion 

Inside DB find and set:
PS_SSL_ENABLED = 0
PS_SHOP_DOMAIN = domain.local
Tables:
ps_shop_url - change domain

Regenerate .htaccess to update links and stuff
shop parameters -> Traffic & SEO 
disable  Friendly URL and enable

Inside prestashop root folder delelte /var/cache !!!!importat may lead error without debug mode 

Edit SMTP settings to prevent emails been sent to customer
NOTE: Gmail smtp settings

Edit profile, make it local 

Check Information tab for configuration issues


``` bash
<VirtualHost *:80>
  ServerName dezomeda.local
  DocumentRoot "/var/www/html/dezomeda.lt/"
  <Directory "/var/www/html/dezomeda.lt/">
    AllowOverride all
  </Directory>
  ErrorLog /var/www/html/logs/dezomeda_error.log
  CustomLog /var/www/html/logs/dezomeda_error_access.log combined
</VirtualHost>
```


**PrestaShop admin login loop fix (local):**

1. In DB set `PS_COOKIE_CHECKIP` to `0`
2. In DB set `PS_COOKIE_SAMESITE` to `Lax`
3. Clear cache: `rm -rf ~/www/dezomeda/var/cache/*/*`