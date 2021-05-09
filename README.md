# Installing and running Symfony demo

> This repo is about installing and running Symfony demo.    

## Usage 
### To run Symfony demo
[https://makuznet-at-gmail-com-sym-demo-php.devops.rebrain.srwx.net/](https://makuznet-at-gmail-com-sym-demo-php.devops.rebrain.srwx.net/)

### To get connected via ssh
Use `REBRAIN.SSH.PUB.KEY` to get connected.
```bash
ssh makuznet-at-gmail-com-sym-demo-php.devops.rebrain.srwx.net -l root
```

## Installation
### Terraform
This is to roll out a VPS in Digital Ocean with DNS in Amazon.
See `main.tf` file for details.
```bash
terraform init
terraform plan
terraform apply --auto-approve
```

### PHP
```bash
sudo apt update
sudo apt install -y php php-fpm php-sqlite3 php-mysql php-mbstring php-dom
```

### Composer
```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
composer
```

### NGINX
```bash
sudo apt install -y nginx
```

### Symfony demo app
The "Symfony Demo Application" is a reference application created to show how to develop applications following the Symfony Best Practices.
```bash
sudo apt install -y zip
cd /var/www/
sudo composer create-project symfony/symfony-demo sym-demo
cd sym-demo
```
sym-demo — /var/www/sym-demo — dir where all the files will be copied to.  

If a Symfony-demo dir was just copied from GitHub it's worth to run:  
```bash
cd sym-demo
composer install
```
and Composer will install everything what is needed.

### MySQL
```bash
sudo apt install -y gnupg
cd /tmp
wget https://dev.mysql.com/get/mysql-apt-config_0.8.17-1_all.deb
sudo dpkg -i mysql-apt-config* # choose mysql 7.5 cluster
sudo apt update
sudo apt install -y mysql-server
```

### Changing database sqlite to mysql
Composer will generate tables automagically.
```bash

sudo mysql
    create database symfony;
    create user 'symfony'@'%' identified by 'netlab';
    grant all privileges on symfony.* to 'symfony'@'%';
    \q

sudo vi /var/www/sym-demo/.env
    DATABASE_URL=mysql://symfony:netlab@127.0.0.1:3306/symfony

sudo mysql
    use symfony; # connecting to the database
    show tables; # supposed to be empty
    \q 

cd /var/www/sym-demo/   

sudo ./bin/console doctrine:schema:create  # creating a DB structure from code  
```
#### Download test data
```bash
sudo ./bin/console doctrine:fixtures:load # loading test data to the DB from code

mysql
    use symfony; # connecting to the database
    select * from symfony_demo_post; # make sure test data has been uploaded to the table
    \q
```

### Letsencrypt
```bash
sudo apt install letsencrypt

# getting an ssl certificate
letsencrypt certonly --webroot -w /var/www/html -d makuznet-at-gmail-com-sym-demo-php.devops.rebrain.srwx.net -m makuznet@gmail.com --agree-tos

# backing up /etc/letsencrypt
scp -r root@makuznet-at-gmail-com-sym-demo-php.devops.rebrain.srwx.net:/etc/letsencrypt ~/Documents/rebrain/db_migration_php/
```

### Configuring NGINX
Run Ansible to copy NGINX configuration files and remove a default symlink.
```bash
ansible-playbook -i inventory.yml main.yml
```

## Acknowledgments

This repo was inspired by [rebrainme.com](https://rebrainme.com) team

## See Also
- [PHP](https://www.php.net/)
- [Composer](https://getcomposer.org)
- [Symfony Demo Application](https://github.com/symfony/demo)
- [Symfony Configuring a Web Server](https://symfony.com/doc/current/setup/web_server_configuration.html#nginx)
- [Nginx 1.4.x on Unix systems](https://www.php.net/manual/en/install.unix.nginx.php)
- [Packagist — package list](https://packagist.org)
- [Composer cheat sheet in Russian](https://phpprofi.ru/blogs/post/52)
- [MySQL Community Downloads](https://dev.mysql.com/downloads/file/?id=504286)

## License
Follow all involved parties licenses terms and conditions.