```
mkdir src

docker-compose build

docker-compose up -d

docker exec -it m2-php bash

cd /var/www/magento246

composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition=2.4.6-p2 .

bin/magento setup:install \
  --base-url=http://localhost/ \
  --db-host=m2-db \
  --db-name=magento246 \
  --db-user=mage246_user \
  --db-password=mage246_pass \
  --admin-firstname=admin \
  --admin-lastname=admin \
  --admin-email=admin@admin.com \
  --admin-user=admin \
  --admin-password=admin123 \
  --language=en_US \
  --currency=USD \
  --timezone=Europe/London \
  --use-rewrites=1 \
  --search-engine=elasticsearch7 \
  --elasticsearch-host=m2-elasticsearch \
  --elasticsearch-index-prefix=magento246 \
  --elasticsearch-port=9200
  
php bin/magento setup:config:set --cache-backend=redis --cache-backend-redis-server=m2-redis --cache-backend-redis-port=6379 --cache-backend-redis-db=0

php bin/magento setup:config:set --page-cache=redis --page-cache-redis-server=m2-redis --page-cache-redis-db=1

php bin/magento setup:config:set --session-save=redis --session-save-redis-host=m2-redis --session-save-redis-port=6379 --session-save-redis-log-level=4 --session-save-redis-db=2

php bin/magento setup:upgrade

php bin/magento setup:di:compile

php -dmemory_limit=6G bin/magento setup:static-content:deploy -f

chmod -R 777 var pub generated app/etc/*
```
To install sample data
```
bin/magento sampledata:deploy
bin/magento setup:upgrade
```