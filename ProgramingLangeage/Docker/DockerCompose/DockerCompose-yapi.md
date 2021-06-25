###dockerComposer
```
version: '3'

services:
  yapi-web:
    image: jayfong/yapi:latest
    container_name: yapi-web
    ports:
      - 3100:3000
    environment:
      - YAPI_ADMIN_ACCOUNT=liuwei@metware.cn
      - YAPI_ADMIN_PASSWORD=metware_admin123!@#
      - YAPI_CLOSE_REGISTER=true
      - YAPI_DB_SERVERNAME=yapi-mongo
      - YAPI_DB_PORT=27017
      - YAPI_DB_DATABASE=yapi
      - YAPI_MAIL_ENABLE=false
      - YAPI_LDAP_LOGIN_ENABLE=false
      - YAPI_PLUGINS=[]
    depends_on:
      - yapi-mongo
    links:
      - yapi-mongo
    restart: unless-stopped
  yapi-mongo:
    image: mongo:latest
    container_name: yapi-mongo
    volumes:
      - ./data/db:/data/db
    expose:
      - 27017
    restart: unless-stopped
```