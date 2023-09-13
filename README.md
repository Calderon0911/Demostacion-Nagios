# Demostracion Nagios 
Porfavor seguir las siguientes configuraciones para obtener la demostracion al 100% funcional. 

#1 actualizamos nuestro server 

```bash
sudo apt update
sudo apt upgrade
```

## 2 Cambiamos a usuario de root para realizar todas las configuraciones.

```bash
sudo -i
```

## 3 instalamos los prerrequisitos 

```bash
apt-get install -y autoconf gcc libc6 make wget unzip apache2 php libapache2-mod-php7.4 libgd-dev
apt-get install openssl libssl-dev
```

## 4 Descargamos Nagios

```bash
cd /tmp
wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.14.tar.gz
tar xzf nagioscore.tar.gz
```

Cambiamos de librería (Importante no salir de esta librería mientras realizamos las siguientes configuraciones)

```bash
cd nagioscore-nagios-4.4.14/
```
```bash
./configure --with-httpd-conf=/etc/apache2/sites-enabled
```
```bash
make all
```

## 5 Creamos el usuario de Nagios y el grupo.

colocamos una contraseña importante no olvidarla.

```bash
make install-groups-users
```
```bash
usermod -a -G nagios www-data
```

## 6 Instalamos los binarios.

```bash
make install
```
```bash
make install-daemoninit
```
```bash
make install-config
```
```bash
make install-commandmode
```
```bash
make install-webconf
```

## 7 Instalamos las configuraciones de apache2

```bash
a2enmod rewrite
a2enmod cgi
```

## 8 Realizamos configuraciones para que el Firewall no genera problemas.

```bash
ufw allow Apache
ufw reload
```
## 8 Reiniciamos servicios y añadimos httpasswd

```bash
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

Añadimos una contraseña

```bash
systemctl restart apache2
```
```bash
systemctl status apache2
```
```bash
systemctl start nagios.service
```

## 9 Añadimos los plugins de Nagios para que todo funcione bien.

```bash
apt-get install -y autoconf gcc libc6 libmcrypt-dev make libssl-dev wget bc gawk dc build-essential snmp libnet-snmp-perl gettext
```
```bash
cd /tmp
```
```bash
wget --no-check-certificate -O nagios-plugins.tar.gz https://github.com/nagios-plugins/nagios-plugins/archive/release-2.4.6.tar.gz
```
```bash
tar zxf nagios-plugins.tar.gz
```

## 10 compilamos e instalamos.

```bash
cd /tmp/nagios-plugins-release-2.4.6/
```
```bash
./tools/setup
```
```bash
./configure
```
```bash
make
```
```bash
make install
```
```bash
systemctl restart nagios.service
```

## 11 Ya podemos ingresar a Nagios por WEB server.

```bash
[Ip de su servidor]/nagios
```






