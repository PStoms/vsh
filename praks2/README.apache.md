# Apache2

## installimine

Apache2 installeerimiseks sisestasin käsu

```
apt-get install apache2
```
peale seda muutsin index.html faili ära.

```
cd /var/www/html
```
```
nano index.html
```
```
<!DOCTYPE HTML>
<html>
<meta charset="UTF-8">
<body>
<p>Patrick Stauning Toms veebileht</p>
<p>ISP217</p>
<p>p.toms@khk.ee
ISP217</p>
</body>

</html>

```
tavakasutajale avaliku kausta tegemiseks kasutasin käsku

```
mkdir /home/it/public_html.

```

seejärel linkisin selle /var/www kaustaga kasutades käsku
```
ln -s /home/it/public_html./ /var/www
```

peale seda lubasin kasutaja kaustad käsuga
```
a2enmod userdir
```
ning tegin apache2 teenusele restarti.
```
service apache2 restart
```
