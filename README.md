# getdrone

基于最新的 Drone 0.8 构建，支持 Coding。

## 如何管理密码/密钥

### 以 SSH 插件为例

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

TODO

## 如何在容器中运行Docker

TODO
