## How to make secure your site - best tips

> Disable directory listing
```
Options All -Indexes
IndexIgnore *
```
> secure directory by disabling script execution
```
AddHandler cgi-script .php .pl .py .jsp .asp .htm .shtml .sh .cgi
Options -ExecCGI
```

> allow all except those indicated here
```
<Limit GET POST PUT>
 Order allow,deny
 Allow from all
 Deny from 122.170.1.34
 #Deny from example\.com
</Limit>
```

> diguise all file extensions as php
``` 
ForceType application/x-httpd-php
```

> protect against DOS attacks by limiting file upload size
```
LimitRequestBody 10240000

<FilesMatch "\.(mov|mp3|jpg|pdf|mp4|avi|wmv)$">
   ForceType application/octet-stream
   Header set Content-Disposition attachment
</FilesMatch>

<Files *.css.gz>
	ForceType text/css
</Files>
<Files *.js.gz>
	ForceType application/javascript
</Files>
<Files *.html.gz>
	ForceType text/html
</Files>

<FilesMatch "\.(?i:js)$">
  ForceType application/javascript
  Header set Content-Disposition attachment
</FilesMatch>
<FilesMatch "\.(?i:css)$">
  ForceType text/css
  Header set Content-Disposition attachment
</FilesMatch>
```

> Change folder name based on request url
```
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} ^(www.)?zbc.net$ [NC]
RewriteRule ^(.*)$ /development/$1 [L]

RewriteCond %{HTTP_HOST} ^xyz\.com$
RewriteRule (.*) http://www.xyz.com/$1 [R=301,L]
RewriteRule ^(.*)$ /production/$1 [L]
```

> Error document
```
ErrorDocument 404 /welcome/404.html
ErrorDocument 500 /error-pages/500.html
ErrorDocument 503 /error-pages/503.html
ErrorDocument 504 /error-pages/504.html
```

> redirect-to-http-non-www-to-https-www-htaccess
```
RewriteEngine On
RewriteCond $1 !^(index\.php|(.*)\.swf|forums|images|css|downloads|jquery|js|robots\.txt|favicon\.ico)
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ ./index.php?$1 [L,QSA]

# we replace domain.com/$1 with %{SERVER_NAME}%{REQUEST_URI}.
RewriteCond %{HTTP_HOST} !^www\. [NC]
RewriteRule ^(.*) https://www.%{SERVER_NAME}%{REQUEST_URI} [L,R=301]

# here we dont use www as non www was already redirected to www.

RewriteCond %{HTTPS} off
RewriteRule ^(.*) https://%{SERVER_NAME}%{REQUEST_URI} [L,R=301]
```

> Allow only specific ip address only
```
RewriteCond %{REMOTE_HOST} !^12.170.1.34
RewriteCond %{REMOTE_HOST} !^29.19.19.58
RewriteCond %{REMOTE_HOST} !^18.13.11.99
RewriteCond %{REMOTE_HOST} !^52.70.30.225
RewriteRule .* http://www.domain.com [R=302,L]
```

### laravel remove public from url
> Copy past .htaccess from inside public folder to root directory and replace below code
```
<IfModule mod_rewrite.c>
    <IfModule mod_negotiation.c>
        Options -MultiViews
    </IfModule>
    
    RewriteEngine On
    
    RewriteCond %{REQUEST_FILENAME} -d [OR]
    RewriteCond %{REQUEST_FILENAME} -f
    RewriteRule ^ ^$1 [N]

    RewriteCond %{REQUEST_URI} (\.\w+$) [NC]
    RewriteRule ^(.*)$ public/$1 

    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ server.php

</IfModule>
```
> Forword non-http to https
```
    RewriteEngine On
    RewriteCond %{HTTP:X-Forwarded-Proto} !https
    RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

> Make all folder permission 755
```
chmod 755 xyz-folder-name
```
> Make all files permission 644
```
find . -iname "*.php" -exec chmod 644 {} \;
```