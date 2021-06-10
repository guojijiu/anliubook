###dockerComposer
```
version: "3.2"

services:
  zentao:
    image: idoop/zentao:latest
    container_name: ZentaoApplication
    restart: always
    environment:
      TZ: 'Asia/Shanghai'
      ADMINER_USER: "root"
      ADMINER_PASSWD: "123456"
      BIND_ADDRESS: "false"
    ports:
      - 10002:80
    volumes:
      - ./data/:/opt/zbox/
```
