run\_dolma\_deduplicate.sh

## 概述
Dolma 是一个用于大规模文本数据预处理的开源工具，其中的 `dedupe` 命令用于文档或段落级别的去重。它使用 Bloom Filter 算法来高效识别重复内容。

## 核心功能
* ​**​文档级去重​**​：基于指定字段（如URL）识别完全重复的文档
* ​**​段落级去重​**​：识别文档内部的重复段落
* ​**​高效处理​**​：使用 Bloom Filter 实现内存高效的去重
* ​**​并行处理​**​：支持多进程加速

## 基本命令
bash

```Plain Text
dolma dedupe \
    --documents "/mnt/wikipedia/v0/documents/*" \
    --dedupe.paragraphs.attribute_name 'bff_duplicate_paragraph_spans' \
    --dedupe.skip_empty \
    --bloom_filter.file /tmp/deduper_bloom_filter.bin \
    --no-bloom_filter.read_only \
    --bloom_filter.estimated_doc_count '6_000_000' \
    --bloom_filter.desired_false_positive_rate '0.0001' \
    --processes 16
```
### 输入输出配置
* `--documents`: 必需参数，指定输入文档路径，支持通配符（如`/path/to/files/*.jsonl`）
* `--work_dir.input`: 可选，指定临时输入文件目录（默认为系统自动创建临时目录）
* `--work_dir.output`: 可选，指定临时输出文件目录（默认为系统自动创建临时目录）

### 去重配置
* `--dedupe.paragraphs.attribute_name`: 必需参数之一，指定段落重复标记的属性名称（如`duplicate_spans`）
* `--dedupe.documents.key`: 文档级去重的JSON路径字段（如`$.metadata.url`），与段落参数互斥
* `--dedupe.documents.attribute_name`: 文档级去重的标记属性名（如`is_duplicate`），与段落参数互斥
* `--dedupe.skip_empty`: 可选标志，跳过空文档/段落
* `--dedupe.min_length`: 可选，设置最小处理字符数（默认0）
* `--dedupe.min_words`: 可选，设置最小处理单词数（默认0）

### 高级段落去重（n-gram模式）
* `--dedupe.paragraphs.by_ngram.ngram_length`: 可选，设置n-gram长度（禁用完整段落检查）
* `--dedupe.paragraphs.by_ngram.stride`: 可选，设置n-gram步长（默认1）
* `--dedupe.paragraphs.by_ngram.threshold`: 可选，设置重复判定阈值（默认1.0）

### Bloom Filter 配置
* `--bloom_filter.file`: 必需参数，指定Bloom Filter存储路径（如`/tmp/bf.bin`）
* `--bloom_filter.size_in_bytes`: 可选，直接指定Bloom Filter大小（字节），与预估参数互斥
* `--bloom_filter.estimated_doc_count`: 必需参数之一，预估处理的文档数量（如`6_000_000`）
* `--bloom_filter.desired_false_positive_rate`: 必需参数之一，期望的误报率（如`0.0001`表示0.01%）
* `--bloom_filter.read_only`: 可选标志，启用只读模式
* `--no-bloom_filter.read_only`: 可选标志，禁用只读模式（默认）

### 性能配置
* `--processes`: 可选，设置并行进程数（默认1）
* `--dryrun`: 可选标志，只验证配置不执行实际操作

### 特殊说明
1. 文档级与段落级参数互斥（只能选择一种去重方式）
2. Bloom Filter大小可通过两种方式指定：

* 直接指定`size_in_bytes`
* 通过`estimated_doc_count`和`desired_false_positive_rate`自动计算3. 输入文件格式：
* 默认假设为gzip压缩格式（`.jsonl.gz`）
* 未压缩文件需显式声明`--compression "none"`

