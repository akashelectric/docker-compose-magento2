# docker-compose-magento2

And our last directory is magento2 directory. In our case we have downloaded Magento 2 latest version from https://meetanshi.com/blog/download-magento/ and unarchive it in magento2 directory. This directory will be mapped with /var/www/html directory in docker container.

Now untar the downloaded magento file named 2.4.2 by using command:

wget https://codeload.github.com/magento/magento2/tar.gz/refs/tags/2.4.2
tar -xvzf 2.4.2

Now move the data extracted in directory magento2-2.4.2 into your magento2 directory

Now run the command to build docker container

docker-compose up -d 

Now go into the container of apache2 by using command:

docker exec -it apache2 bash

Run command composer and check the version of the composer if the composer is not installed the run the command for installing composer

composer
composer install

For checking the database is created or not use command:

Mysql -u root -ppassword -h 127.0.0.1
SHOW DATABASES;

If the magentodb named database is created then leave it as it is if the database is not created then create it by using the command:

CREATE DATABASE 	magentodb;
EXIT;

Enable elasticsearch before installing the magento by command:

/etc/init.d/elasticsearch restart

Now run the main command to install the magento2:

bin/magento setup:install \
--base-url=http://43.205.215.1 \
--db-host=127.0.0.1 \
--db-name=magentodb \
--db-user=root \
--db-password=password \
--admin-firstname=admin \
--admin-lastname=admin \
--mailto:admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin123 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1


Now give permissions to the files:

[permissions]:

find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +

find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +

chown -R :www-data .

chmod -R 777 var/ setup/ vendor/ pub/ bin/

chmod u+x bin/magento

php bin/magento c:c

php bin/magento c:f

php bin/magento indexer:reindex

Now go to /etc/apache2/sites-available/000-default.conf and add /pub after the html in document root directory 

Now restart the apache2 by using the command:

/etc/init.d/apache2 restart

Now hit the server by using your ip !!!!!!!
