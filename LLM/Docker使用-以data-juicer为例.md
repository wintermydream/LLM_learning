#### 拉取官方镜像​​
docker pull datajuicer/data-juicer:latest

#### 运行容器
run\_container.sh

```python
docker run -dit \
  --privileged \
  --shm-size 32g \
  --network host \
  --gpus all \
  --rm \
  --name DataJuicer_wu \
  -v $PWD:/workspace \
  -v /mnt/zzb/peixunban/zzb6/data/ShengdunWu:/mnt \
  -v ~/.cache/:/root/.cache/ \
  datajuicer/data-juicer:latest \
  /bin/bash
```
```python
chmod +x run_container.sh
./run_container.sh
```
#### 进入容器
docker exec -it DataJuicer\_wu bash



### **验证是否成功​**​
1. ​**​检查容器运行状态​**​：

```Plain Text
docker ps -a | grep DataJuicer_wu
```
* ​**​测试GPU支持​**​（进入容器后执行）：

```Plain Text
nvidia-smi  # 应显示GPU信息
python -c "import torch; print(torch.cuda.is_available())"  # 应输出True
```
* ​**​检查挂载目录​**​：

```Plain Text
ls /workspace  # 应显示主机当前目录内容
ls /mnt        # 应显示主机/mnt挂载的数据
```
---
#### 退出容器
1.  退出容器但保持容器运行​​

    方法​​：按下 Ctrl+P，然后按 Ctrl+Q（需连续按，中间无停顿）

    ​​效果​​：退出交互终端，但容器继续在后台运行

2. 退出并停止容器​

    ​​方法​​：在容器内输入 exit 或按下 Ctrl+D

    ​​效果​​：退出终端并停止容器的主进程（若主进程为当前Shell，容器会停止）

3. 强制终止容器（从宿主机操作）​​

```python
docker stop DataJuicer_wu  # 优雅停止
docker kill DataJuicer_wu  # 强制终止
```
