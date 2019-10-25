# PRACTICA 01
## https://github.com/antoniobm1/practica001.git

### Script con el cual instalamos

#!/bin/bash
# Actualizamos los repositorios
```
apt-get update
```

# Instalamos apache
```
apt-get install apache2 -y
```

# Instalamos paquetes para apache
```
apt-get install php libapache2-mod-php php-mysql -y
```

# Instalamos phpmyadmin
```
apt install php libapache2-mod-php php-mysql
```

# Creamos un archivo llamado info-php en el directorio /var/www/html

```
nano /var/www/html/info.php
```

# Añadimos el siguiente contenido
```
<? php
phpinfo ();
?>
```

# Instalamos adminer
```
cd /var/www/html
mkdir adminer
cd adminer
wget https://github.com/vrana/adminer/releases/download/v4.3.1/adminer-4.3.1-mysql.php 
mv adminer-4.3.1-mysql.php index.php
```

# Instalamos MYSQL Server
```
apt-get install mysql-server -y
```

# Comprobamos que tiene bien la pass de Mysql con: 
```
mysql -u root -p
```

# Para la instalacion del GoAccess necesitamos añadir los repositorios
```
echo "deb http://deb.goaccess.io/ $(lsb_release -cs) main" | tee -a /etc/apt/sources.list.d/goaccess.list

wget -O - https://deb.goaccess.io/gnugpg.key | apt-key add -
```
```
apt-get update
apt-get install goaccess
```

# Con esto ya estaria instalado el GoAccess

# Control de acceso a un directorio con .htaccess
## Crea un directorio llamado stats dentro del directorio /var/www/html

## Paso 1 creamos el directorio stats.
```
mkdir /var/www/html/stats
```

## Paso 2 Comenzamos el proceso de goaccess para generar informes en 2º plano.
```
goaccess /var/log/apache2/access.log -o /var/www/html/stats/index.html --log-format=COMBINED --real-time-html &
```

## Paso 3 Creamos el archivo de contraseñas para el usuario que accedera al directorio stats.
```
htpasswd -c /home/usuario/.htpasswd usuario
```

## Paso 4 Creamos el archivo .htaccess en el directorio que queremos proteger con usuario y contraseña.
```
nano /var/www/html/stats/.htaccess
```

### El contenido del archivo .htaccess será el siguiente:
```AuthType Basic
AuthName "Restricted Content"
AuthUserFile /home/usuario/.htpasswd
Require valid-user
```

## Paso 5  Editamos el archivo configuración de Apache.
nano /etc/apache2/sites-enabled/000-default.conf

### Añadimos la siguiente sección dentro de las etiquetas de <VirtualHost *:80> y </VirtualHost>.
```<Directory "/var/www/html/stats">
Options Indexes FollowSymLinks
AllowOverride All
Require all granted
</Directory>
```


### Paso 6 Reiniciamos el servicio de Apache
```
/etc/init.d/apache2 restart
```
