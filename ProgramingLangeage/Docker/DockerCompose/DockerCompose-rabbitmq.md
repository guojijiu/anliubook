###dockerComposer
```
version: '3.2'
services:
    rabbitmq3:
        build: .
        ports:
            - 15672:15672
            - 5672:5672
        environment:
            RABBITMQ_DEFAULT_USER: ${RABBITMQ_USER}
            RABBITMQ_DEFAULT_PASS: ${RABBITMQ_PASS}
        volumes:
            - "./db:/var/lib/rabbitmq"
           # - "./conf:/etc/rabbitmq/"
           # - "./logs:/var/log/rabbitmq/log"
        restart: always
        container_name: rabbitmq3

```
