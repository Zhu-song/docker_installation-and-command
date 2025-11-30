# 一、Docker 基础命令（环境检查 / 服务控制）

|命令|功能说明|示例|
|---|---|---|
|docker --version|查看 Docker 版本|`docker --version` → 输出 `Docker version 27.0.3, build 7d4bcdc`|
|docker info|查看 Docker 系统信息（镜像数、容器数、内核版本等）|`docker info` → 显示镜像仓库、存储驱动、网络模式等详情|
|docker --help|查看所有 Docker 命令帮助|`docker --help` 或 `docker 子命令 --help`（如 `docker run --help`）|
|`systemctl start docker`（Linux）|启动 Docker 服务|需 root 权限，CentOS/Ubuntu 通用|
|`systemctl stop docker`（Linux）|停止Docker服务|sudo systemctl stop docker|
|`systemctl restart docker`（Linux）|重启 Docker 服务|配置修改后需重启生效|
|`systemctl enable docker`（Linux）|设置 Docker 开机自启|避免重启服务器后手动启动|
|`open -a Docker`（Mac）|启动 Docker 应用|Mac 需先启动 Docker 桌面端才能执行其他命令|

# 二、镜像管理命令（拉取 / 构建 / 删除 / 推送）

```
镜像是容器的模板，核心操作围绕「获取 - 管理 - 分发」展开：
```

|命令|功能说明|示例|
|---|---|---|
|docker pull [镜像名]:[标签]|从镜像仓库拉取镜像（默认 Docker Hub）|拉取 CentOS 7 镜像：`docker pull centos:7`；拉取最新版：`docker pull nginx:latest`（标签可省略，默认 latest）|
|`docker images` / `docker image ls`|列出本地所有镜像|`docker images` → 显示镜像 ID、标签、大小等|
|docker rmi [镜像ID/镜像名]|删除本地镜像|删除 nginx 镜像：`docker rmi nginx:latest`；强制删除（需先删依赖容器）：`docker rmi -f nginx:latest`|
|docker image prune|删除所有未被容器使用的虚悬镜像（dangling images）|释放磁盘空间：`docker image prune -f`（`-f` 强制删除，无需确认）|
|docker build -t [镜像名]:[标签] [Dockerfile路径]|基于 Dockerfile 构建镜像|在 Dockerfile 所在目录执行：`docker build -t my-nginx:v1 .`（`.` 表示当前目录）|
|docker tag [原镜像名]:[原标签] [新镜像名]:[新标签]|给镜像打标签（用于版本管理 / 推送仓库）|给本地镜像打标签推送到私有仓库：`docker tag nginx:latest my-registry/nginx:v1`|
|docker push [镜像名]:[标签]|推送镜像到远程仓库（Docker Hub / 私有仓库）|推送前需登录：`docker login`，再执行 `docker push my-nginx:v1`|
|docker search [关键词]|搜索 Docker Hub 上的镜像|搜索 Python 镜像：`docker search python` → 显示镜像星级、描述、是否官方镜像|

# 三、容器操作命令（创建 / 启动 / 停止 / 删除 / 进入）

|命令|功能说明|示例|
|---|---|---|
|docker run [参数] [镜像名] [容器内命令]|创建并启动容器（最常用命令）|1. 后台启动 nginx 并映射 80 端口：`docker run -d -p 80:80 --name my-nginx nginx`；  <br>  <br>2. 交互式启动 CentOS（进入命令行）：`docker run -it --name my-centos centos:7 /bin/bash`；-  <br>  <br>3. 启动容器并自动删除（测试用）：`docker run --rm nginx echo "hello docker"`|
|关键参数说明|-d：后台运行（ detached 模式）；  <br>-it：交互式终端（可进入容器操作）；  <br>-p 主机端口：容器端口：端口映射；  <br>--name：给容器命名（避免用容器 ID 操作）；  <br>-v 主机目录：容器目录：数据卷挂载（持久化数据）；  <br>--restart=always：容器退出后自动重启||
|docker ps|列出正在运行的容器|`docker ps` → 显示容器 ID、名称、端口映射、状态等|
|docker ps -a|列出所有容器（运行中 + 已停止）|查看历史创建的容器：`docker ps -a`|
|docker start [容器ID/容器名]|启动已停止的容器|启动名为 my-nginx 的容器：`docker start my-nginx`|
|docker stop [容器ID/容器名]|停止运行中的容器|停止 my-nginx：`docker stop my-nginx`；强制停止：`docker kill my-nginx`（适用于无法正常停止的容器）|
|docker restart [容器ID/容器名]|重启容器|配置修改后重启：`docker restart my-nginx`|
|docker rm [容器ID/容器名]|docker rm [容器ID/容器名]|删除已停止的 my-centos：`docker rm my-centos`；强制删除运行中的容器：`docker rm -f my-centos`|
|**docker exec -it [容器ID/容器名] [命令]**|**进入运行中的容器并执行命令（常用交互）**|**进入 my-nginx 容器的 bash 终端：`docker exec -it my-nginx /bin/bash`；执行单次命令（不进入终端）：`docker exec my-nginx ls /usr/share/nginx/html`**|
|docker logs [容器ID/容器名]|查看容器日志|实时查看 nginx 日志：`docker logs -f my-nginx`（`-f`持续输出）；查看最近 100 行：`docker logs --tail 100 my-nginx`|
|docker inspect [容器ID/容器名/镜像名]|查看容器 / 镜像的详细配置（IP、挂载、环境变量等）|查看 my-nginx 的 IP 地址：`docker inspect my-nginx|
|docker container prune|删除所有已停止的容器|批量清理无用容器：`docker container prune -f`|

# 四、数据卷管理命令（持久化数据）

|命令|功能说明|示例|
|---|---|---|
|docker volume create [卷名]|创建自定义数据卷|创建名为 nginx-data 的卷：`docker volume create nginx-data`|
|docker volume ls|列出所有数据卷|`docker volume ls` → 显示卷名、驱动类型|
|docker volume inspect [卷名]|查看数据卷详情（挂载路径等）|`docker volume inspect nginx-data` → 显示主机上的实际存储路径（如 `/var/lib/docker/volumes/nginx-data/_data`）|
|docker run -d -v [卷名]:[容器目录] [镜像名]|启动容器时挂载数据卷|挂载 nginx-data 到容器的 `/usr/share/nginx/html`：`docker run -d -v nginx-data:/usr/share/nginx/html -p 80:80 --name my-nginx nginx`|
|docker volume rm [卷名]|删除数据卷（需先解绑容器）|`docker volume rm nginx-data`|
|docker volume prune|删除所有未被容器使用的数据卷|`docker volume prune -f` → 释放磁盘空间|

# 五、网络管理命令（容器网络配置）

```
Docker 网络支持桥接、host、容器互联等模式，常用命令：
```

|命令|功能说明|示例|
|---|---|---|
|docker network ls|列出所有 Docker 网络|默认网络：bridge（桥接）、host（主机模式）、none（无网络）|
|docker network create [网络名]|创建自定义网络（推荐用自定义网络实现容器互联）|创建名为 my-network 的桥接网络：`docker network create my-network`|
|docker run -d --network [网络名] --name [容器名] [镜像名]|启动容器时加入指定网络|启动两个容器在同一网络（可通过容器名互通）：  <br>`docker run -d --network my-network --name app1 nginx`  <br>`docker run -d --network my-network --name app2 centos:7` → app1 可 ping 通 app2（`ping app2`）|
|docker network connect [网络名] [容器名]|将已启动的容器加入网络|把已运行的 app3 加入 my-network：`docker network connect my-network app3`|
|docker network disconnect [网络名] [容器名]|断开容器与网络的连接|docker network disconnect my-network app3|
|docker network rm [网络名]|删除自定义网络（需先断开所有容器）|docker network rm my-network|
|docker network prune|删除所有未被容器使用的网络|docker network prune -f|

# 六、常用组合命令（日常高频场景）

1. **批量停止 / 删除容器**：  
    停止所有运行中的容器 `docker stop $(docker ps -q)`  
    删除所有已停止的容器 `docker rm $(docker ps -aq)`  
    强制删除所有容器（无论是否运行） `docker rm -f $(docker ps -aq)`
2. **批量删除镜像**：  
    删除所有本地镜像`docker rmi -f $(docker images -q)`  
    删除所有虚悬镜像（标签为 的镜像） `docker rmi -f $(docker images -f "dangling=true" -q)`  
    3.**容器数据备份（复制容器文件到主机）**：  
    把容器内的 /usr/share/nginx/html 目录复制到主机的 ./backup 目录 `docker cp my-nginx:/usr/share/nginx/html ./backup`  
    4.**主机文件复制到容器**:  
    把主机的 ./index.html 复制到容器的 /usr/share/nginx/html 目录 `docker cp ./index.html my-nginx:/usr/share/nginx/html`