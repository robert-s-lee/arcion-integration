
```bash
docker network create arcnet

docker volume create mautic_mysql

docker run --name mysql-mautic -d \
    --restart=always \
    -p :3306 \
    -e MYSQL_ROOT_PASSWORD=password \
    -v mautic_mysql:/var/lib/mysql \
    --net=arcnet \
    percona/percona-server:5.7 \
     --character-set-server=utf8mb4 \ --collation-server=utf8mb4_general_ci \
     --default-authentication-plugin=mysql_native_password \
    --local-infile=true

docker volume create mautic_data

docker run --name mautic -d \
    --restart=always \
    -e MAUTIC_DB_HOST=mysql \
    -e MAUTIC_DB_USER=arcsrc \
    -e MAUTIC_DB_PASSWORD=password \
    -e MAUTIC_DB_NAME=arcsrc \
    -e MAUTIC_RUN_CRON_JOBS=true \
    -p :80 \
    --net=arcnet \
    -v mautic_data:/var/www/html \
    mautic/mautic:v3
 ```   

 ```bash
 docker exec -it mautic mysqldump -hmysql -uarcsrc -ppassword --no-data test > dump-defs.sql
 ```