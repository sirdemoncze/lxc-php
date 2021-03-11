# Instalace
1. Prioritní balíčky
```
apt-get -y install software-properties-common apt-transport-https lsb-release ca-certificates locales
```

2. Kódování prostředí
```
localedef -i cs_CZ -c -f UTF-8 -A /usr/share/locale/locale.alias cs_CZ.UTF-8
```

3. Nastavení zdroje pro PHP (sury)
```
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
```

4. Instalace Webového serveru, PHP a ostatních balíčků
```
apt-get install -y \
htop \
mc \
socat \
mariadb-server \
apache2 \
php7.3 \
php7.3-common \
php7.3-json \
php7.3-zip \
php7.3-mysql \
php7.3-gd \
php7.3-imap \
php7.3-ldap \
php7.3-pgsql \
php7.3-pspell \
php7.3-tidy \
php7.3-curl \
php7.3-xmlrpc \
php7.3-xsl \
php7.3-bz2 \
php7.3-mbstring \
php7.3-bcmath \
php7.3-dba \
php7.3-soap \
php7.3-imagick \
php7.3-memcache \
php7.3-mysql \
php7.3-sybase \
php7.3-sqlite3 \
php7.3-ssh2
```

5. Zakázání spouštění MariaDB serveru
```
update-rc.d mysql disable
```

6. Nastavení služeb
Zkopírovat složku /etc/ z repozitáře do containeru a následně povolit služby.
```
systemctl enable etc-apache2.mount etc-php.mount home-hosting.mount mysql-socket-redirection mysql-tcp-redirection
```

7. Povolení vzdáleného SSH přihlášení na root
```
sed -i 's/^#\?\(PermitRootLogin \).*/\1yes/' /etc/ssh/sshd_config
systemctl sshd reload
```
