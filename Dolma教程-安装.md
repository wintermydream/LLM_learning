使用dolma的 docker：

```python
#直接导入镜像
docker import olmo_0820.tar my-olmo-image:0422

#部署容器
docker run -dit \
  --privileged \
  --shm-size 32g \
  --network host \
  --gpus all \
  --rm \
  --name Dolma_wu \
  -v $PWD:/workspace \
  -v /mnt/zzb/peixunban/zzb6/data/ShengdunWu:/mnt \
  -v ~/.cache/:/root/.cache/ \
  my-olmo-image:0422 \
  /bin/bash

# 启动容器
docker exec -it Dolma_wu bash
```
或者自己安装，存在依赖包冲突的问题，虚拟环境能解决部分问题

```python
# 创建单独的dolma虚拟环境
conda create --name env_dolma
# 安装
pip install dolma  # 或源码安装：
pip install -e .[all] -i https://pypi.tuna.tsinghua.edu.cn/simple
```
