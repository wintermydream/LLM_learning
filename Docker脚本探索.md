#### 直接导入镜像
docker import olmo\_0820.tar my-olmo-image:0422



#### 进入容器
docker run -it --rm --gpus all --name olmo-gpu-container my-olmo-image:0422 /bin/bash



**问题**：docker: Error response from daemon: could not select device driver "" with capabilities: \[\[gpu\]\].

**解决**：**安装 NVIDIA Container Toolkit​**​

​**​步骤1：添加仓库​**​

```Plain Text
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
&& curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
&& curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
​**​步骤2：安装工具包​**​

```Plain Text
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
```
​**​步骤3：重启 Docker​**​

```Plain Text
sudo systemctl restart docker
```


**重新运行你的容器​**​

```Plain Text
docker run -it --rm --gpus all --name olmo-gpu-container my-olmo-image:0422 /bin/bash
```


#### 挂载文件夹
创建一个脚本文件（如 `run_container.sh`）：

```Plain Text
#!/bin/bash
docker run -dit \
--privileged \
--shm-size 32g \
--network host \
--gpus all \
--rm \
--name Docker_wu \
-v $PWD:/workspace \
-v /home/wu/LLM/data:/mnt \
-v ~/.cache/:/root/.cache/ \
my-olmo-image:0422 \
/bin/bash
```
赋予执行权限并运行：

```Plain Text
chmod +x run_container.sh
./run_container.sh
```
进入容器

```python
docker exec -it Docker_wu bash
```
检查文件夹

```python
ls -l /workspace  # 检查当前目录挂载
ls -l /mnt        # 检查数据目录挂载
ls -l /root/.cache # 检查缓存挂载
```
### 编写代码
对/workspace的文件操作，编写代码

#### 方法1：直接使用 VS Code 的「Remote - Containers」扩展（推荐）​​
​**​步骤1：安装必备插件​**​

1. 在 VS Code 中安装官方扩展：Remote - Containers

​**​步骤2：连接容器​**​

1. 打开 VS Code 的命令面板（`Ctrl+Shift+P`）
2. 输入并选择：Remote-Containers: Attach to Running Container...
3. 从列表中选择你的容器 `Docker_wu`

​**​步骤3：访问挂载目录​**​
在 VS Code 左侧资源管理器中：
`/workspace` → 对应主机当前目录（`$PWD`）
`/mnt` → 对应主机 `/home/wu/LLM/data`2. 直接编辑文件，修改会自动同步到主机和容器

---
#### ​**​**方法2：通过本地挂载目录编辑（适合纯本地开发）​​​
1. 直接在主机上用 VS Code 打开挂载目录：
2. 所有修改会自动同步到容器内（因目录已通过 `-v` 挂载）

