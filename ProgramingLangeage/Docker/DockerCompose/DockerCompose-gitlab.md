###dockerComposer
```
version: '3.2'
services:
    gitlab:
        image: gitlab/gitlab-ce
        container_name: gitlab
        restart: always
        environment:
            TZ: 'Asia/Shanghai'
            GITLAB_OMNIBUS_CONFIG: |
                external_url 'http://121.36.77.88:8929/gitlab'
        ports:
            - '8929:8929'
            - '11022:22'
        volumes:
            - "./config:/etc/gitlab"
            - "./logs:/var/log/gitlab"
            - "./data:/var/opt/gitlab"

```
