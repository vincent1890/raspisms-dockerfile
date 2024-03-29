YAML example :
--------------

version: "3"
services:
  raspisms:
    image: raspisms:latest
    container_name: raspisms
    restart: unless-stopped
    depends_on:
      - db
    environment:
      - LOCAL_DATE=fr-FR
      - TZ=Europe/Paris
      - PUID=1000
      - PGID=1000
      - APP_SECRET=$CONFIG_SECRET
      - APP_HTTP_PROTOCOL=http
      - APP_STATIC_HTTP_URL=http://URL:PORT
      - APP_DATABASE_HOST=raspisms-db
      - APP_DATABASE_NAME=raspisms
      - APP_DATABASE_USER=$DATABASE_USER
      - APP_DATABASE_PASS=$DATABASE_PASS
      - CREATE_ALL_SETTING=true
    volumes:
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
      - $CONFIG_RASPISMS_PATH:/config
    devices:
      - /dev/ttyUSB0:/dev/modem
    ports:
      - 80:80
    networks:
      - sms
      
  db:
    image: mariadb
    container_name: raspisms-db
    restart: unless-stopped
    volumes:
      - $CONFIG_MARIADB_PATH:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=$DATABASE_ROOT_PASS
      - MYSQL_DATABASE=raspisms
      - MYSQL_USER=$DATABASE_USER
      - MYSQL_PASSWORD=$DATABASE_PASS
    networks:
      - sms
      
      
networks: 
  sms:
    external: true

