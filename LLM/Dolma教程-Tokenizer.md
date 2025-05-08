## 核心功能
使用 GPT-NeoX-20B 的分词器对文本数据进行分词处理，为后续大模型训练准备数据。

## 关键参数说明
* `--documents`: 输入文件路径（支持通配符，如 `*.gz`）
* `--tokenizer.name_or_path`: 使用的分词器名称/HuggingFace路径
* `--destination`: 分词后输出目录
* `--processes`: 并行处理进程数

## 代码示例
bash

```Plain Text
# 基本用法（处理gzip压缩的文档）
dolma tokens \
    --documents "/path/to/input/documents/*.gz" \
    --tokenizer.name_or_path "EleutherAI/gpt-neox-20b" \
    --destination /path/to/output/tokens \
    --processes 16

# 实际生产环境示例（处理Wikipedia数据）
dolma tokens \
    --documents "/mnt/wikipedia/example0/documents/*.gz" \
    --tokenizer.name_or_path "EleutherAI/gpt-neox-20b" \
    --destination /mnt/wikipedia/example0/tokens \
    --processes 16
```
## 技术要点
1. ​**​分词器特性​**​:

* GPT-NeoX-20B 分词器基于 BPE 算法
* 词表大小: 50,257
* 支持最大序列长度: 20482. ​**​性能优化​**​:
* 使用多进程并行处理（`--processes`参数）
* 自动处理压缩文件（如.gz格式）3. ​**​输出格式​**​:
* 输出为二进制token序列
* 保持原始文档结构

## 使用建议
1. 对于大规模数据处理：

bash

```Plain Text
# 使用更多进程提高吞吐量
--processes $(nproc)  # 使用所有可用CPU核心
```
2. 监控资源使用：

bash

```Plain Text
# 结合GNU parallel控制资源
parallel -j 4 dolma tokens ::: --documents /path/to/batch_{1..4}/*.gz
```
3. 验证分词结果：

python

```Plain Text
from transformers import AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained("EleutherAI/gpt-neox-20b")
sample = "Hello world! This is a test."
print(tokenizer.tokenize(sample))
```
## 注意事项
1. 输入文件需为纯文本或gzip压缩文本
2. 输出目录需有足够存储空间（原始文本的1.2-1.5倍）
3. 处理前确保分词器缓存已下载（默认在\~/.cache/huggingface）

