### **解压并查看文件结构​**​
bash

```Plain Text
gzip -d your_file.json.gz  # 解压（生成 your_file.json）
head -n 1 your_file.json   # 查看第一行样本
```
* Dolma 通常要求 ​**​每行一个 JSON 对象​**​（JSON Lines 格式），类似：

```Plain Text
{"id": "doc1", "text": "example text...", "metadata": {...}}
```
---
### ​**​2. 验证关键字段​**​
Dolma 的输入格式通常需要以下字段（具体以官方文档为准）：

* ​**​必需字段​**​：
* `id`: 唯一文档标识符（字符串）。
* `text`: 文档正文内容（字符串）。\* ​**​可选字段​**​：
* `metadata`: 包含来源、作者等信息的字典（如 `{"source": "web", "timestamp": "2023-01-01"}`）。

用 `jq` 工具快速检查字段是否存在：

```Plain Text
head -n 1 your_file.json | jq 'has("id"), has("text")'
```
输出应为 `true`。

不解压高效检查JSON文件的结构:

```python
zcat wiki-0000.json.gz | head -n 100 | jq -c 'has("id"), has("text"), has("metadata")' | sort | uniq
```
---
### ​**​3. 检查编码和特殊字符​**​
bash

```Plain Text
file your_file.json      # 确认编码（如 UTF-8）
less your_file.json      # 手动检查特殊字符（如乱码）
```


