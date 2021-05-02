# Postgresql migration PHP

> This repo creates Postgresql migration with PHP.   

Composer - Dependency Manager for PHP.
The main file — `composer.json`.
 gem file for Ruby
requirements.txt for Jango
packages.json for Node.js
pom.xml for Java

## Usage 
### Run Composer
```bash
composer
```
### Run builtin Composer web server
```bash
./bin/console server:run
```
### Creating an ssh tunnel to get remotely run Composer web server locally 
```bash
ssh remote.host.ip.addr -l root -L 8000:127.0.0.1:8000
```
### Run Symfony demo project
Insert in your web browser
```bash
http://localhost:8000
```

## Installation
#### Composer for Linux (from video: v1.7.3)
- [Download Composer](https://getcomposer.org/download)

### Symfony demo (video: v1.2.7)
The "Symfony Demo Application" is a reference application created to show how to develop applications following the Symfony Best Practices.

- [Symfony Demo Application](https://github.com/symfony/demo)

```bash
cd /var/www
composer create-project symfony/symfony-demo demo
```
demo — /var/www/demo — dir where all the files will be copied to

If a Symfony-demo dir was just copied from GitHub it's worth to run
```bash
cd demo
composer install
```
and Composer will install everything wha is needed.

There is `.env` configuration file where next settings are located:
- APP_ENV=dev
- APP_DEBUG=1
- APP_SECRET=67d829bf61dc5f87a73fd814e2c9f629
- DATABASE_URL=sqlite:///%kernel.projects_dir%/data/database.sqlite
- MAILER_URL=null://localhost

### Changing database sqlite to mysql
Composer will generate tables automagically.
```bash
apt install mysql-server
mysql
    create database symfony;
    create user 'symfony'@'%' identified by '123456';
    grant all privileges on symfony.* to 'symfony'@'%';
    \q
vi /var/www/demo/.env
    DATABASE_URL=mysql://symfony:123456@127.0.0.1:3306/symfony
mysql
    user symfony;
    show tables;
    \q
apt install php7.2-mysql    
./bin/console doctrine:schema:create    

```
#### Download test data
```bash
/bin/console doctrine:fixtures:load
mysql
    select * from symfony_demo_post; # make sure test data has been uploaded to the table
```
## Acknowledgments

This repo was inspired by [rebrainme.com](https://rebrainme.com) team

## See Also
- [PHP](https://www.php.net/)
- [Composer](https://getcomposer.org)
- [Symfony Demo Application](https://github.com/symfony/demo)
- [Nginx 1.4.x on Unix systems](https://www.php.net/manual/en/install.unix.nginx.php)

## License
Follow all involved parties licenses terms and conditions.