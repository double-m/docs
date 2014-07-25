## Symfony HowTo

### Mapping a virtual host on the working copy using Apache HTTP Server 2.4

Reference: [http://symfony.com/get-started](<http://symfony.com/get-started>)

As `username`, install Symfony using Composer (as explained): during
the installation, create `Me/MyProjectBundle`. As `root`, edit `/etc/hosts` so that:

```
root@linuxbox:~# cat /etc/hosts | grep myproject
127.0.0.1       myproject.local www.myproject.local
```

Create a new virtual host according with the new hostname:

```
root@linuxbox:~# cat << EOF | tee /etc/apache2/sites-available/myproject.conf
<VirtualHost *:80>
    ServerName myproject.local
    ServerAlias www.myproject.local

    DocumentRoot /home/username/path/to/symfony/src/myproject/web
    <Directory /home/username/path/to/symfony/src/myproject/web>
        # enable the .htaccess rewrites
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog /var/log/apache2/myproject_error.log
    CustomLog /var/log/apache2/myproject_access.log combined
</VirtualHost>
EOF
root@linuxbox:~# cd /etc/apache2/sites-enabled
root@linuxbox:~# ln -s /etc/apache2/sites-available/myproject.conf
```

One way to avoid permission conflicts between your `username` and
`www-data` in accessing `.../app/cache` and `.../app/logs` is to run
Apache as `username` and not as the default user `www-data`:

```
root@linuxbox:~# cat /etc/apache2/envvars | grep APACHE_RUN
export APACHE_RUN_USER=username
export APACHE_RUN_GROUP=username
export APACHE_RUN_DIR=/var/run/apache2$SUFFIX
```

Restart Apache:

```
root@linuxbox:~# /etc/init.d/apache2 restart
```

Check:

- [http://www.myproject.local/hello/marcello](<http://www.myproject.local/hello/marcello>) (production version)
- [http://www.myproject.local/app_dev.php/hello/marcello](<http://www.myproject.local/app_dev.php/hello/marcello>) (development version - more logging)
- [http://www.myproject.local/app_dev.php](<http://www.myproject.local/app_dev.php>) (ACME Bundle)
