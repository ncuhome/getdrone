# getdrone

基于最新的 Drone 0.8 构建，支持 Coding。

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

## 如何在容器中运行Docker

TODO
