###dockerComposer
```
version: '3.2'
services:
    jenkins:
        image: jenkins/jenkins:lts
        volumes:
            - "./data/:/var/jenkins_home"
            - "/home/deploy/docker/www:/docker/www:rw"
            - "/usr/local/bin/php:/usr/local/bin/php"
            - "/usr/bin/composer:/usr/bin/composer"
        ports:
            - "8280:8080"
        expose:
            - "8280"
            - "50000"
        privileged: true
        user: root
        restart: always
        container_name: jenkins
        environment:
            - TZ=Asia/Shanghai

```
