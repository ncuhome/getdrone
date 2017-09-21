# getdrone

基于最新的 Drone 0.8 构建，支持 Coding。

## 如何配置和运行

修改 `docker-compose.yml` 中相应的占位符内容，使用 `docker-compose` 运行。

参考：
- http://docs.drone.io/installation/
- http://docs.drone.io/release-0.8.0

## 如何管理密码/密钥

### 以 SSH 插件为例

http://plugins.drone.io/appleboy/drone-ssh/

在 Drone 项目的 `Secrets` 页面，添加 `ssh_key`，值为密码或密钥。

CI 容器中添加 `secrets: [ ssh_key ]`:

```yaml
pipeline:
    ssh:
        image: appleboy/drone-ssh
        pull: true
        host: SERVER_IP
        username: dev
        port: 22
        secrets: [ ssh_key ]
        script:
            - echo hello
            - echo world
```

原理：
Drone 会将 `ssh_key` 的值设为容器的环境变量 `SSH_KEY`，容器內的进程即可读取这个值。

## 如何使用缓存

http://plugins.drone.io/drillster/drone-volume-cache/

### 以Pip为例

在 Drone 项目的 `Settings` 页面，设置 `Trusted` 为选中状态（仅管理员可操作）。

```yaml
restore-cache:
    image: drillster/drone-volume-cache
    restore: true
    mount:
        - .cache
    volumes:
        - /tmp/cache:/cache
update_all:
    image: python:3.6
    commands:
        - pip install --cache-dir=.cache -r requires.txt -i https://pypi.douban.com/simple
        - fab update_all
rebuild-cache:
    image: drillster/drone-volume-cache
    rebuild: true
    mount:
        - .cache
    volumes:
        - /tmp/cache:/cache
```

## 如何在Docker中运行Docker

在 Drone 项目的 `Settings` 页面，设置 `Trusted` 为选中状态（仅管理员可操作）。

```yaml
docker:
    image: docker
    volumes:
        - /var/run/docker.sock:/var/run/docker.sock
    commands:
        - docker version
        - docker ps
```

PS: 限制较多，不支持挂载容器中的目录作为数据卷。

## 如何构建此项目

参考 https://github.com/drone/drone 源码中的 `.drone.sh` 脚本，
手动 build 出 release 目录下两个可执行文件。


## 注意事项

1. Secrets里面加的ssh_key必须是本地的私钥（别写成服务器的私钥了）