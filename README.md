# Instalace LXC PHP
1. Aktualizace stávající instalace
```
apt-get update
apt-get upgrade -y
```

2. Prioritní balíčky
```
apt-get install -y software-properties-common apt-transport-https lsb-release ca-certificates locales chrony
```

2. Časové zóny a kódování prostředí
```
dpkg-reconfigure tzdata
localedef -i cs_CZ -c -f UTF-8 -A /usr/share/locale/locale.alias cs_CZ.UTF-8
```

3. Nastavení zdroje pro PHP (sury)
```
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get update
```

4. Instalace Webového serveru, PHP a ostatních balíčků
```
apt-get install -y \
    htop \
    mc \
    socat \
    nfs-common \
    mariadb-server \
    apache2 \
    php7.4 \
    php7.4-common \
    php7.4-json \
    php7.4-zip \
    php7.4-mysql \
    php7.4-gd \
    php7.4-imap \
    php7.4-ldap \
    php7.4-pgsql \
    php7.4-pspell \
    php7.4-tidy \
    php7.4-curl \
    php7.4-xmlrpc \
    php7.4-xsl \
    php7.4-bz2 \
    php7.4-mbstring \
    php7.4-bcmath \
    php7.4-dba \
    php7.4-soap \
    php7.4-imagick \
    php7.4-memcache \
    php7.4-mysql \
    php7.4-sybase \
    php7.4-sqlite3 \
    php7.4-ssh2
```

5. Zakázání spouštění MariaDB serveru
```
systemctl disable mariadb
```

6. Nastavení služeb
Zkopírovat složku /etc/ z repozitáře do containeru a následně povolit služby.
```
systemctl enable etc-apache2.mount etc-php.mount home-hosting.mount mysql-socket-redirection mysql-tcp-redirection
```

7. Povolení vzdáleného SSH přihlášení na root
```
sed -i 's/^#\?\(PermitRootLogin \).*/\1yes/' /etc/ssh/sshd_config
systemctl reload sshd
```

8. Nastavení aktuální složky při ukončení Midnight Commander
```
echo "alias mc='source /usr/lib/mc/mc-wrapper.sh'" >> ~/.profile
. ~/.profile
```

9. Nastavení individuálních nastavení pro MSSQL
```
echo "
[lk]
host = 80.188.134.210
port = 1433
tds version = 8.0
client charset = UTF-8" >> /etc/freetds/freetds.conf
```

10. Lokální DNS
``
echo "
# SQL
192.168.254.32	sql

# Back compatibility
127.0.0.1	sparrow.compsys.cz
127.0.0.1	sqlserver.datnet.cz
127.0.0.1	sql.datnet.cz
127.0.0.1	sqlserver.vendys.net
127.0.0.1	sql.vendys.net" >> /etc/hosts
``


## Přesměrování fce mail() (postix relay)
1. Instalace prioritních balíčků
```
apt-get install libsasl2-modules
```

2. Úprava souboru main.cf
```
relayhost = [mail]:587
smtp_tls_loglevel=1
smtp_tls_security_level=encrypt
smtp_sasl_auth_enable=yes
smtp_sasl_security_options = noanonymous
smtp_sasl_password_maps = hash:/etc/postfix/relay_passwd
```

3. Vytvoření souboru pro ověření
```
echo "[mail]:587 <uzivatelske_jmeno>:<heslo>" > /etc/postfix/relay_passwd
postmap /etc/postfix/relay_passwd
```

