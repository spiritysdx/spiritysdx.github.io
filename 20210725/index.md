# Okteto常用项目

# docker-compose搭建

## wordpress

```yaml

version: '3.3'
 
services:
   db:
     image: mysql:latest
     volumes:
       - /data/wordpress_db:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: rootwordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress
 
   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
```

## Cloudreve

```yaml
services:
    cloudreve:
        public: true
        container_name: cloudreve
        image: jialezi/cloudreve
        ports:
            - 5212:5212
        volumes: 
            - /root/cloudreve
```

## Bitwarden

管理地址为“xxx/admin”。

```yaml
services:
    bit:
        public: true
        container_name: bitwarden
        image: bitwardenrs/server:alpine
        ports:
            - 80:80
        volumes: 
            - /data/
        environment:
            - WEBSOCKET_ENABLED=true
            - SIGNUPS_ALLOWED=true
            - WEB_VAULT_ENABLED=true
            - ADMIN_TOKEN=%管理员Token自己填%
```

## 青龙面板
```yaml
version: '2.0'
services:
  ql:
    image: whyour/qinglong:latest
    container_name: ql
    restart: always
    volumes:
       - /qinglong/ql/config:/ql/config
       - /qinglong/ql/scripts:/ql/scripts
       - /qinglong/ql/repo:/ql/repo
       - /qinglong/ql/log:/ql/log
       - /qinglong/ql/db:/ql/db
       - /qinglong/ql/jbot:/ql/jbot
       - /qinglong/ql/raw:/ql/raw
    ports:
       - 5801:5700
```

## docker简易管理面板
```yaml
version: '3'
services:
  redis:
    image: redis:latest
  web:
    image: registry.cn-hangzhou.aliyuncs.com/seven-tao/simple-docker:0.0.7
    ports:
      - "9091:4050"
    volumes:
      - /tmp/simple-docker/back:/tmp/back
      - /var/run/docker.sock:/var/run/docker.sock
    depends_on:
      - redis
```

# chart搭建

### tensorflow-notebook

改动密码部署即可，内置jupyter-notebook。

### terminal

可以通过命令行安装额外的东西，就是给权限很麻烦。
