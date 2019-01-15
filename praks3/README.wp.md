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
Järgmisena läksin phpmyadmin lehele sisestades 172.23.13.30/phpmyadmin ja logisin
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

# HTTPS konfigureerimine apache2 veebiserverile

## Võtme genereerimine
Kõigepealt lõin endale uue kausta kuhu ma oma sertifikaadid panin ```mkdir /etc/apache2/ssl```
Siis genereerisin endale sertifikaadi 1095 päevaks ehk kolmeks aastaks käsuga ```openssl req -x509 -nodes -days 1095 -newkey rsa:2048 -out /etc/apache2/ssl/server.crt -keyout /etc/apache2/ssl/server.key```
Mis omakorda näitas järgnevat
```
Generating a 2048 bit RSA private key
............................................+++
.....................+++
writing new private key to '/etc/apache2/ssl/server.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:EE
State or Province Name (full name) [Some-State]:Tartu
Locality Name (eg, city) []:Tartu
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:172.23.13.30
Email Address []:
```
Kõige tähtsam koht selles asjas oli ```Common Name``` kus tuli määrata oma veebilehe aadress. Mina panin sinna oma veebilehe ip aadressi
## Faili konfigureerimine ja linkimine
Installisin SSL modi apachele käsuga 
```
a2enmod ssl
```

Siis linkisin kaks asja omavahel

```
ln -s /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-enabled/000-default-ssl.conf
```

Nüüd avasin konfigureerimist vajava faili 

```
nano /etc/apache2/sites-enabled/000-default-ssl.conf
```

Ja muutsin need read ära selliseks
```
SSLCertificateFile    /etc/apache2/ssl/server.crt
SSLCertificateKeyFile /etc/apache2/ssl/server.key
```

Siis tegin apachele restarti ja asi töötas 
```/etc/init.d/apache2 restart```

Tõestus töötavast asjast on siin
        vsh/Hõiva.PNG
