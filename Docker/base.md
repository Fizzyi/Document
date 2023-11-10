# Docker

## 一、镜像相关

- 下载镜像：`docker pull mysql/mysql-server`
- 查看镜像：`docker images`
- 删除指定镜像： `docker rmi`
- 构建镜像：`docker build`

## 二、容器相关

- 创建并且运行容器  `docker run`
- 停止容器：`docker stop`
- 启动容器：`docker start`  启动被停止的容器，不会新建容器
- 删除容器： `docker rm`
- 查看所有容器：`docker ps`
- 查看容器日志：`docker logs`
- 进入容器内部：`docker exec`

### 2.1、创建容器

`docker run --name mysql-local -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql/mysql-server`

- - -name 容器命名
- - p 将容器里面的端口（后）暴露给宿主机（前）
- - d 后台运行
- -v 目录挂载需要使用绝对路径 -v D:/code:/app 将app目录下的数据挂在在D:/code目录下，这样只需要修改D：/code下的文件，容器里面的文件就会同时修改，避免经常build。可以将日志文件保存在D：/code目录下。
- 最后是指定镜像

### 2.7进入容器内部

`docker exec -it 容器ID /bin/bash`

## 三、 数据卷

理解就是在容器外挂载的文件或者文件夹，这样当需要修改配置文件之类的时候只需要修改宿主机上的文件就行，不需要去修改容器内部的文件。

将容器内的目录和文件与宿主机的目录和文件进行关联，称为挂载。

挂载是在 容器 创建时使用 -v 参数进行挂载。

```jsx
// 挂载本地目录
-v 本地目录:容器内目录
// 挂载本地文件
-v 本地文件:容器内文件
```

如果不挂载数据卷的话，一些存储文件，例如 mysql 的数据之类的，当容器销毁后，数据就会销毁。

## 四、Dockerfile

自动打包镜像的功能。

常用的命令：

- FROM  指定基础镜像
- ENV 设置环境变量，可在后面指令中使用
- COPY 拷贝本地文件到镜像的指定目录
- RUN 执行 Linux 的 shell 命令，一般是安装过程的命令
- EXPOSE 指定容器运行时监听的端口，是给镜像使用者看的
- ENTRYPOINT 镜像中应用的启动命令，容器运行时调用

`docker build -t 镜像名称:tag .`

- -t 参数是指定镜像的名称和tag
- . 是指构建时 Dockerfile 所在路径，.代表当前目录，也可以直接指定 Dockerfile 目录。

## 五、网络

常见命令：

- docker network create：创建一个网络
- docker network ls: 查看所有网络
- docker network rm：删除所有网络
- docker network prune: 清除未使用的网络
- docker network connect：使指定容器链接加入某网络
    
    `docker network connect 网络名称 容器名 - - alias 别名`，该网络内的其他容器可以用别名相互访问 
    
- docker network disconnect：使指定容器连接离开某网络
- docker network inspect：查看网络详细信息

在自定义网络中，可以给容器起多个别名，默认的别名是容器名本身

在同一个自定义网络中的容器，可以通过别名互相访问

## 六、DockerCompose

实现多个相互关联的 Docker 容器的快速部署，通过一个单独的 docker-compose.yml文件来定义一组相关联的应用容器。

与 docker run 对比

| docker run 参数 | docker compose 参数 | 说明 |
| --- | --- | --- |
| - - name | container_name | 容器名称 |
| -p | ports | 端口映射 |
| -e | environment | 环境变量 |
| -v | volumes | 数据卷配置 |
| —network | networks | 网络 |
|  | image | 镜像名称 |
|  | commad | 执行命令 |

```yaml
version: 自定义版本号

services:
	mysql:
		image: mysql
		container_name: mysql
		ports:
			- "3306:3306"
		environment:
			TZ: Asia/Shanghai 
		volumes:
			- "./mysql/conf:/etc/mysql/conf.d"
		networks:
			- hm-net

networks:
	hm-net:
		name: haml
	
```

基本语法： `docker compose [OPTIONS] [COMMAND]`

| 类型 | 参数/指令 | 说明 |  |
| --- | --- | --- | --- |
|  OPTIONS | -f | 指定 compose 文件的路径和名称 |  |
|  | -p | 指定 project 名称， |  |
| COMMAND | up | 创建并启动所有的 service 容器 | -d 参数是后台启动 |
|  | down | 停止并移出所有容器、网络 | -v 是用于删除相关的匿名卷 |
|  | ps | 列出所有启动的容器 |  |
|  | logs | 查看指定容器的日志 |  |
|  | stop | 停止容器 |  |
|  | start | 启动容器 |  |
|  | restart | 重启容器 |  |
|  | top | 查看运行的进程 |  |
|  |  exec | 在指定的运行容器中执行命令 |  |
|  |  |  |  |