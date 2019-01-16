# Andmebaasi server
## Paigaldus
Andmebaasi serverile paigaldasin mariadb-server ja alustasin mysql installationi.
```
apt install mariadb-server

mysql_secure_installation
```

mysql installationis  kõigele peale parooli vahetuse yes.



Järgmisena muutsin /etc/mysql/my.cnf failis bind-address rea ära 
andmebaasi serveri ip vastu, milleks oli 10.0.2.4

Tegin mysql teenusele restarti

Sisenesin mysqli ning sisestasin käsud

```

CREATE DATABASE wordpress;
CREATE USER 'WPuser'@'localhost' IDENTIFIED BY 'qwerty';
GRANT ALL PRIVILEGES ON wordpress.* TO 'WPuser'@'localhost';
CREATE USER 'WPuser'@'10.0.2.4' IDENTIFIED BY 'qwerty';
GRANT ALL PRIVILEGES ON wordpress.* TO 'WPuser'@'10.0.2.4';
exit

```


# wordpressi server

Wordpressi paigaldamiseks tegin süsteemi uuenduse ja paigaldasin apache2 ning mariadb-client ja php-mysql.
Paigaldasin ka zip käsu

Sisestasin sources.listi 

```
deb http://packages.dotdeb.org jessie all
deb-src http://packages.dotdeb.org jessie all

```

Tegin ```apt install php7.0-*```

peale seda tegin ```service apache2 reload```.

Läksin /tmp kausta ning laadisin alla wordpressi ja pakkisin selle lahti.

```
cd /tmp
wget -c http://wordpress.org/latest.zip
unzip -q latest.zip -d /var/www/html/
```

Määrasin õigused ja tegin vajalikud kaustad.

```
chown -R www-data.www-data /var/www/html/wordpress
chmod -R 755 /var/www/html/wordpress
mkdir -p /var/www/html/wordpress/wp-content/uploads
chown -R www-data.www-data /var/www/html/wordpress/wp-content/uploads
```
```
cd /var/www/html/wordpress/
cp wp-config-sample.php wp-config.php
nano wp-config.php
```

wp-config.php failis muutsin ära useri, passwordi, database name ja ka hostname. hostname kohale panin 10.0.2.4


# klient

kliendi arvutile paigaldasin ubuntu 16.04 ja määrasin sellele 2GB RAM ja 10GB ketast.
