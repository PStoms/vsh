# PHP ja MYSQL moodulid

## paigaldus

php moodulite paigaldamiseks sisenesin /etc/apt/sources.list faili ja lisasin read:

```
deb http://packages.dotdeb.org jessie all
deb-src http://packages.dotdeb.org jessie all
```

Peale seda ma tegin uuendused ja tõmbasin php moodulid mis on vajalikud veebiserveri pagaldadamiseks.


```
apt-get update
```

```
apt-get install php7.0-cli php7.0-curl php7.0-dev php7.0-zip 
php7.0-fpm php7.0-gd   php7.0-xml php7.0-mysql php7.0-mcrypt php7.0-mbstring php7.0-opcache
```

## mysql paigaldamine

/tmp kausta tuli laadida mysql konfiguratsioni
```
wget https://dev.mysql.com/get/mysql-apt-config_0.8.9-1_all.deb
```

Pakkisin selle lahti käsuga
```
dpkg -i mysql-apt-config_0.8.9-1_all.deb
```
konfiguratsioonis valisin
versiooni 5.6

Peale seda tegin süstemi uuenduse ja tõmbasin mysql-community-server, mille abil määrasin mysql root kontole parooli.
```
apt-get update 
```
```
apt-get install mysql-community-server
```
mysqli käsureal sisselogimiseks ilma paroolita tegin root kasutaja kodukausta faili .my.cnf kuhu sisse panin
```
[mysql]
user=root
password=qwerty
```

## phpmyadmin install

paigaldasin myphpadmini lehe. Paroolideks määrasin kõikial qwerty
```
apt install phpmyadmin
```
järgmisena tegin apache2.conf faili
```
nano /etc/apache2/apache2.conf
```
lisasin rea Include /etc/phpmyadmin/apache.conf
```
Include /etc/phpmyadmin/apache.conf
```
 ja tegin apache teenusele restarti.
```
systemctl restart apache2 
```
Järgmisena läksin phpmyadmin lehele sisestades 172.23.13.49/phpmyadmin ja logisin
sisse kontoga root ja parooliga qwerty

# php lehe tegemine

algselt tegin index.php faili, kuhu panin algse lehe. 
php info kuvamiseks tegin /var/www/html kausta faili nimega test.php

faili sisestasin rea
```
<?php phpinfo(); ?>
```


kasutajale oma lehe saamiseks tegin public_html kausta test.php faili, kuhu sisestasin tavalise teksti ning php käsuks panin 
```
<?php echo '<p> Kasutaja enda leht </p>'; ?>
```

kuna apache-l on defaultist keelatud php moodulid siis muutsin /etc/apache2/mods-enabled/php7.0.conf failis userdirmodule read commentiga ära.


kõik vajalikud failid ja kuvatõmmised on olemas praks3 kaustas.
