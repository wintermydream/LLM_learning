### 传文件
在 Linux 内网环境下，使用 scp（Secure Copy Protocol）可以高效、安全地传输文件到指定 IP 的指定路径。

```python
# 将本地的 /home/user/data.txt 传到 192.168.1.100 的 /opt/backup/
scp /home/user/data.txt wu@10.15.8.150:/home/wu/LLM/
# 文件夹
scp -r tokens root@10.200.98.120:/mnt/zzb/peixunban/zzb6/data/ShengdunWu/

scp -r 4_5 wu@10.15.8.150:/home/wu/LLM/
```


#### 查看本机名和IP
查看本地用户名： whoami

查看本地IP​​： ip a



### 查看机器配置
**1\. 查看物理 CPU 核心数​**​

```Plain Text
grep "physical id" /proc/cpuinfo | sort -u | wc -l
```
​**​2. 查看每个物理 CPU 的核心数​**​

```Plain Text
grep "cpu cores" /proc/cpuinfo | uniq
```
​**​3. 查看逻辑 CPU 数（总线程数）​**​

```Plain Text
nproc      # 直接显示逻辑 CPU 总数
```


### 文件压缩
**方法 1：使用 ****gzip**** 直接压缩（单线程）​，原文件会被删除**​

```Plain Text
gzip *.jsonl
```
---
​**​方法 2：并行压缩（最快方案：**多线程版 gzip**）​**​

```Plain Text
sudo apt install pigz  # 如果未安装（Ubuntu/Debian）
cd documents
find . -name "*.jsonl" -type f | xargs -P $(nproc) -I {} pigz -k {}
```
​**​参数说明​**​：

* `-P $(nproc)`：使用所有 CPU 核心并行压缩
* `-k`：保留原文件（如需删除原文件则去掉此参数）
* `pigz` 速度比普通 `gzip` 快 5-10 倍

---
​**​方法 3：保留原文件的压缩​**​

```Plain Text
find . -name "*.jsonl" -type f -exec bash -c 'gzip -c "{}" > "{}.gz"' \;
```




