## DO NOT USE SSH TO DO THIS SCRIPT IT WILL FAIL

## Compatible with Ubuntu 18.04 and 20.04

Potentially 16.04 also but untested.

#### Open Terminal:


```sudo su```

```nano /etc/apt/sources.list```

```deb http://debian.opennms.org/ stable main```

```wget -O - https://debian.opennms.org/OPENNMS-GPG-KEY | apt-key add -```

If you dont like it saying:

"Conflicting distribution: http://debian.opennms.org stable Release (expected stable but got opennms-26)"

Do this instead: ```deb http://debian.opennms.org/ opennms-26 main``` But be aware that when opennms-27 or futher comes out it will not auto check that. Being on "stable" will but will show and error on each apt update. 

```apt-get update```

```apt-get install oracle-java8-installer -y```

#### (Dont ever run OpenJDK with minecraft clients or servers, it works but the GC is different and is generally frowned upon and some plugins dont get along with it.)

```apt install curl -y```

### !WARNING! Set your FQDN to "localhost" during next step!

```curl https://raw.githubusercontent.com/SudoUser-Sam/Multicraft-Install-Script/master/multicraftserver.sh > multicraftserver.sh&&bash ./multicraftserver.sh```


### After the script is over leave it open where it shows the passwords for the panel and daemon and open a new terminal

```sudo nano /etc/apache2/apache2.conf```

press f6

Search for "Directory /var/www"

Change "Options Indexes FollowSymLinks" to "Options FollowSymLinks"

Also change "AllowOverride None" to "AllowOverride All"


Example of what it should look like:

```
<Directory /var/www/>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
```
```sudo service apache2 restart```

To get php-mcrypt:

```sudo apt-get -y install gcc make autoconf libc-dev pkg-config```

```sudo apt-get -y install php7.4-dev```

```sudo apt-get -y install libmcrypt-dev```

Once the dependencies have been installed, you can install mcrypt with this command:

```sudo pecl install mcrypt-1.0.4```

Then you need to add extension=mcrypt.so to your php.ini file:

```sudo nano /etc/php/7.4/apache2/php.ini```

press f6

search for "Dynamic Extensions"

Delete one of the examples and add:

extension=mcrypt.so

Example of what it should look like:

```
;;;;;;;;;;;;;;;;;;;;;;
; Dynamic Extensions ;
;;;;;;;;;;;;;;;;;;;;;;

; If you wish to have an extension loaded automatically, use the following
; syntax:
;
extension=mcrypt.so
;
; For example:
;
```

Ctrl + X Enter to save

```sudo service apache2 restart```

### Open your browser of choice and go to 127.0.0.1/multicraft/install.php

Video example:

https://youtu.be/ohv8QXDd8Do?t=270

### After you finish multicraft will inform you to delete the install.php file:

```sudo rm -r /var/www/html/multicraft/install.php```

### After that it is smart to change this password:

```sudo nano /home/minecraft/multicraft/multicraft.conf``` 

#### EXAMPLE:

```
##The connection password for deamon communication
## !! Change this when you set Multicraft to listen on a public IP !!
## The same password will have to be used on the panel side in the file: 
## protected/config/config.php
password= YOURPASSWORD
```

### Change it on the panel config file also:

```sudo nano /var/www/html/multicraft/protected/config/config.php```

```
'daemon_password' => 'YOUR PASSWORD',
```

If phpmyadmin does not want to load run this:

```sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf```

```sudo a2enconf phpmyadmin```

```sudo systemctl reload apache2```

Done.
