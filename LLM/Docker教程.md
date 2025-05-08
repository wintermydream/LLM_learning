快速安装docker

```python
sudo apt-get update && sudo apt-get install docker.io
sudo systemctl start docker
sudo docker run hello-world  # 验证安装
```


### 使用
#### 运行程序
Docker 允许你在容器内运行应用程序， 使用 docker run 命令来在容器内运行一个应用程序。

```python
docker run ubuntu:15.10 /bin/echo "Hello world"
```
Docker 以 ubuntu15.10 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello world"，然后输出结果。

#### 运行交互式的容器
通过 docker 的两个参数 -i -t，让 docker 运行的容器实现"对话"的能力：

```python
runoob@runoob:~$ docker run -i -t ubuntu:15.10 /bin/bash
root@0123ce188bd8:/#
```
* **\-t:** 在新容器内指定一个伪终端或终端。
* **\-i:** 允许你对容器内的标准输入 (STDIN) 进行交互。

注意第二行 root@0123ce188bd8:/#，此时我们已进入一个 ubuntu15.10 系统的容器

运行 exit 命令或者使用 CTRL+D 来退出容器

#### 启动容器（后台模式）
```python
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
```
确认容器有在运行，可以通过 **docker ps** 来查看：

使用 docker logs 命令，查看容器内的标准输出：

```python
docker logs 2b1b7a428627
```
使用 **docker stop** 命令来停止容器



### 导出和导入容器
**导出容器**

如果要导出本地某个容器，可以使用 **docker export** 命令。

```Plain Text
$ docker export 1e560fca3906 > ubuntu.tar
```
导出容器 1e560fca3906 快照到本地文件 ubuntu.tar。



**导入容器快照**

可以使用 docker import 从容器快照文件中再导入为镜像，以下实例将快照文件 ubuntu.tar 导入到镜像 test/ubuntu:v1:

```Plain Text
$ cat docker/ubuntu.tar | docker import - test/ubuntu:v1
```
此外，也可以通过指定 URL 或者某个目录来导入，例如：

```Plain Text
$ docker import http://example.com/exampleimage.tgz example/imagerepo
```
#### 查看容器
```python
docker images 命令列出本地所有镜像：
docker ps -a 命令列出所有容器（包括运行中和已停止的）：
```
#### 删除容器
```Plain Text
# 先删除容器
docker rm -f 1e560fca3906
```
下面的命令可以清理掉所有处于终止状态的容器。

```Plain Text
$ docker container prune
```
#### 删除镜像
```python
# 然后删除镜像
docker rmi hello-world
```


#### 容器更新
```python
# 创建Dockerfile： 
FROM my-olmo-image:0422  # 基于原镜像
RUN pip install --upgrade torch==2.1.0 tensorflow==2.12.0  # 指定版本

# 或者使用版本工具
# COPY requirements.txt .
# RUN pip install -r requirements.txt  # 文件内指定版本

# 构建新镜像：
docker build -t my-olmo-image:202406-updated .
```
