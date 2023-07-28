
# Hướng dẫn cài đặt laravel bằng máy ảo ubuntu
NOTE: FILE _setup.sh_ + _default_ (NGINX CONFIG) ĐƯỢC ĐẶT TRONG THƯ MỤC GỐC CỦA LARAVEL
_Yêu cầu cấu hình_
Ubuntu server 22 + laravel10 + php8.1 + mysql + nginx
Vào máy ảo tạo thư mục chứa mã nguồn
```sh
sudo mkdir /srv/laravel-8.x
sudo chmod -R 777 /srv/laravel-8.x
```
Copy mã nguồn vào máy ảo
Tạo file setup.sh
```hs
#!/bin/bash

sudo apt update
sudo apt install -y nginx
# sudo nginx -t
# sudo systemctl start nginx
# sudo systemctl stop nginx
# sudo systemctl reload nginx
# sudo systemctl restart nginx
# sudo systemctl disable nginx
# sudo systemctl enable nginx

sudo apt update
sudo apt install --no-install-recommends php8.1
sudo apt-get install -y php8.1-cli php8.1-common php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath php8.1-fpm

curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
HASH=`curl -sS https://composer.github.io/installer.sig`
echo $HASH
php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer

sudo apt update
sudo apt install -y mysql-server

# sudo apt update
# sudo apt install redis-server
# sudo systemctl start mysql.service

sudo cp default /etc/nginx/sites-available/
sudo systemctl reload nginx
sudo chmod -R 777 storage/
composer install
ip a
```
Tạo database và user
```sh
sudo mysql

# create database laravel
mysql>CREATE USER 'sammy'@'localhost' IDENTIFIED BY 'password';
mysql>GRANT ALL PRIVILEGES ON laravel.* TO 'sammy'@'localhost' WITH GRANT OPTION;

mysql>CREATE USER 'tien'@'ip_may_host' IDENTIFIED BY 'password';
mysql>GRANT ALL PRIVILEGES ON laravel.* TO 'tien'@'ip_may_host' WITH GRANT OPTION;

mysql>FLUSH PRIVILEGES;
mysql>exit;

sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
# bind-address :127.0.0.1 => 0.0.0.0 => mục đích có thể truy cập vào db từ bất kỳ địa chỉ nào
sudo systemctl stop mysql.service
sudo systemctl start mysql.service
```
Tạo file _default_
```sh
server {
    listen 80;
    listen [::]:80;
    server_name laravel-8.x;
    root /srv/laravel-10.x/public;
 
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
 
    index index.php;
 
    charset utf-8;
 
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
 
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
 
    error_page 404 /index.php;
 
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
 
    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
