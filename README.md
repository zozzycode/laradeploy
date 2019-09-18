#1. LOCAL Composer install
https://getcomposer.org/download/
Download and run Composer-Setup.exe

#2. LOCAL my dev projects folder
e:\GitHub\
launch GitBush
Choose your laravel version I use laravel 5.8
https://laravel.com/docs/5.8
create new project:
composer create-project --prefer-dist laravel/laravel blog "5.8.*"

#3.Create GitHub new Repo
https://github.com/zozzycode/
delete readme.md in the LOCAL LARAVEL_PROJECT_FOLDER/
echo "# laradeploy" >> README.md
git init
git config --global core.autocrlf true
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/zozzycode/laradeploy.git
git push -u origin master

LOCAL create LARAVEL_PROJECT_FOLDER/.cpanel.yml
##########file start
---
deployment:
  tasks:
    - export DEPLOYPATH=/home/sfycfswq/
    - /bin/cp -r * $DEPLOYPATH/laradeploy/  #copy all from root
#    - /bin/cp -r ./public/* $DEPLOYPATH/zozzycode.com/laradeploy/
    - /bin/cp ./public/js/*.js $DEPLOYPATH/laradeploy.zozzycode.com/js/
    - /bin/cp ./public/css/*.css $DEPLOYPATH/laradeploy.zozzycode.com/css/
##########file end

LOCAL edit LARAVEL_PROJECT_FOLDER/app/AppServiceProvider.php

Add row
use Illuminate\Support\Facades\Schema;

Add row to the method
public function boot()
{
    Schema::defaultStringLength(191);
}

ADD all project files
git config --global core.autocrlf true
git add .
git commit -m "project files"
git push

#4 HOSTPRO_SETUP
(NEED .cpanel.yml file from STEP3 in the githup project root folder!!!)
https://documentation.cpanel.net/display/CKB/Guide+to+Git+-+Deployment
Create CPANEL/Setup Git™ Version Control
Clone URL: https://github.com/zozzycode/laradeploy.git
Manage/Refresh

#5. HOSTING. COPY ONCE from root/LARAVEL_PROJECT_FOLDER/*.* to the website root/PUBLIC_HTML/
edit PUBLIC_HTML/index.php add path to your project folder in the root website folder
in my case LARAVEL_PROJECT_FOLDER = laradeploy/
row 24 - require __DIR__.'/../LARAVEL_PROJECT_FOLDER/vendor/autoload.php';
row 38 - $app = require_once __DIR__.'/../LARAVEL_PROJECT_FOLDER/bootstrap/app.php';

#6 https://docs.jelastic.com/php-composer-via-ssh
install composer on the server via SSH
Connect with PuTTY (using ppk key)
curl -sS https://getcomposer.org/installer | php
mkdir ~/bin
mv ~/composer.phar ~/bin/composer

for Apache PHP:
export PATH=$PATH:/var/www/bin
echo 'export PATH=$PATH:/var/www/bin' >> ~/.bashrc

for NGINX PHP:
export PATH=$PATH:/var/lib/nginx/bin
echo 'export PATH=$PATH:/var/lib/nginx/bin' >> ~/.bashrc

composer about

#7 add PHP extention
Show your main and additional php.ini files
php --ini

Configuration File (php.ini) Path: /opt/alt/php72/etc
Loaded Configuration File:         /opt/alt/php72/etc/php.ini
Scan for additional .ini files in: /opt/alt/php72/link/conf
Additional .ini files parsed:      /opt/alt/php72/link/conf/alt_php.ini

nano /opt/alt/php72/link/conf/alt_php.ini

add to alt_php.ini last row
extension=fileinfo.so
F2 and ENTER

#8 HOSTPRO DEPLOY
Go CPANEL/Setup Git™ Version Control/Pull or Deploy
Deploy HEAD Commit

#9 Create DB, USER

#10 HOSTING Using File Manager COPY .env.example 
FROM repositiry/LARAVEL_PROJECT_FOLDER 
TO .env your LARAVEL_PROJECT_FOLDER

#10 Using PuTTY go to your LARAVEL_PROJECT_FOLDER
(default .ENV for Laravel here: https://github.com/laravel/laravel/blob/master/.env.example)

composer install

php artisan key:generate
this command will add
APP_KEY=

#11. Using File Manager EDIT .env file:
APP_NAME=YOUR_APP_NAME
APP_ENV=production

DB_DATABASE=hosting_db_name (sfycfswq_laradeploy)
DB_USERNAME=hosting_db_user (sfycfswq_laradeploy_user)
DB_PASSWORD=YOUR_DBUSER_PASSWORD

#12 using PuTTY in LARAVEL_PROJECT_FOLDER
artisan commands
php artisan migrate

#13 create DB, USER locally and add to local .env file
APP_ENV=loacal

DB_DATABASE=local_db_name
DB_USERNAME=local_db_user
DB_PASSWORD=YOUR_DBUSER_PASSWORD

#14 local 
php artisan serve


WORKFLOW STEPS:
1.Loacal code editing.
2.git push.
3.HOSTRPO.
CPANEL/Setup Git™ Version Control/Pull or Deploy/Pull
CPANEL/Setup Git™ Version Control/Pull or Deploy/Pull
4.Deploy HEAD Commit.
5.Online checking cnanges.


