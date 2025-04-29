### 要求
1. 根据提供的txt格式的url，自动下载数据文件
```
cat dolma/urls/v_1.7.txt | xargs -n 1 -P 8 wget -c -P /home/wu/LLM/data_local/dolma_selected_data
```


2. 数据下载后，需要检查数据是否下载完整
```
# 检查文件完整性（CRC校验）
find /home/wu/LLM/data_local/dolma_selected_data/documents -name "*.gz" -type f -exec gzip -t {} \; 2> error.log

# 重新下载损坏文件
grep "corrupt" error.log | awk -F: '{print $1}' | xargs -n 1 -P 8 wget -c -P /home/wu/LLM/data_local/dolma_selected_data/documents
```