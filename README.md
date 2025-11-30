# Docker官方安装包 
# 解决Docker国内网络问题

24年6月以来，大量Docker镜像网站停服，Docker无法下载安装<br>
本仓库致力于解决国内网络原因无法使用Docker的问题。<br>

### 特点：
- 使用Github Action将官网的安装脚本/安装包定时下载到本项目Release，供国内使用<br>
- 官方安装包，安全可靠<br>
- 每天自动定时同步，保证最新<br>

作者：**[技术爬爬虾](https://github.com/tech-shrimp/me)**<br>
B站，抖音，Youtube全网同名，转载请注明作者<br>
# docker安装
## 1.1 Linux
一键安装命令
```
sudo curl -fsSL https://get.docker.com| bash -s docker --mirror Aliyun
```
备用命令（每天自动从官网定时同步）
```
sudo curl -fsSL https://github.com/tech-shrimp/docker_installer/releases/download/latest/linux.sh| bash -s docker --mirror Aliyun
```
> 备用2（如果Github访问不了，可以使用Gitee的链接）<br>
```
sudo curl -fsSL https://gitee.com/tech-shrimp/docker_installer/releases/download/latest/linux.sh| bash -s docker --mirror Aliyun
```
启动docker
```
sudo systemctl start docker
```
开机自启
```
sudo systemctl enable docker
```
## 1.2 Mac
进入项目的Release，下载Mac系统的安装包<br>
https://github.com/tech-shrimp/docker_installer/releases
![](images/mac安装包.png)
注意区分CPU架构类型 Intel芯片选择x86_64, 苹果芯片选择arm64<br>
下载好双击安装即可
## 1.3 Windows
任务栏搜索功能，启用"适用于Linux的Windows子系统" + "虚拟机平台" <br>
![](images/windows功能.png)
管理员权限打开命令提示符，安装wsl2<br>
```
wsl --set-default-version 2
wsl --update --web-download
```
等待wsl安装成功
![](images/wsl2成功.png)
下载Windows版本安装包，进入此项目的Release<br>
https://github.com/tech-shrimp/docker_installer/releases
下载Windows版本安装包
![](images/windows安装包.png)
双击安装即可
>可选:
如果想自己指定安装目录，可以使用命令行的方式
参数 --installation-dir=D:\Docker可以指定安装位置
```
start /w "" "Docker Desktop Installer.exe" install --installation-dir=D:\Docker
```
