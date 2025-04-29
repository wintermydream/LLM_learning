对 ​**​原始文档​**​ 进行多维度标记（Tagging），生成结构化元数据，为后续的 ​**​数据过滤、分割和清洗​**​ 提供依据。

​**​标记阶段（Tagger）​**​

* 通过`cld2_en_paragraph_with_doc_score_v2`等语言检测器，标记每个段落的语言（如是否为英语），并计算文档级的英语置信度分数（例如“80%段落是英语”）。
* 输出结果保存在`attributes/<experiment>`目录下，包含原始文档的标记信息（如语言标签、分数）

**过滤阶段（后续步骤）​**​
用户需要根据标记结果​**​自行编写过滤逻辑​**​，例如：
* 删除英语置信度低于某阈值（如`<0.7`）的文档。
* 结合其他`tagger`（如`char_length_v1`）进一步筛选。\* 过滤通常在数据预处理流水线的后续步骤完成（如使用脚本或Dolma的其他工具处理`attributes`文件）。

### 1\. ​**​基本功能​**​
`tagger`可以对文档进行多种操作，例如：

* ​**​语言检测​**​：使用`cld2`或`fastText`检测文档或段落的语言。
* ​**​长度统计​**​：计算文档或段落的字符数、token数等。
* ​**​内容过滤​**​：标记包含敏感信息（如PII）、版权声明或有害内容的段落。
* ​**​随机标记​**​：为文档分配随机数，便于数据集划分（如训练集、验证集和测试集）。

### 2\. ​**​内置Tagger​**​
Dolma提供了多种内置的`tagger`，例如：

* ​**​语言相关​**​：`cld2_en_doc_v2`、`ft_lang_id_en_doc_v2`等。
* ​**​长度统计​**​：`char_length_v1`、`whitespace_tokenizer_v1`等。
* ​**​内容过滤​**​：`pii_regex_v1`、`code_secrets_v1`等。
* ​**​随机标记​**​：`random_number_v1`。

完整的列表可以通过运行`dolma list`命令获取

。

### 3\. ​**​使用方式​**​
`tagger`通过`dolma tag`命令运行，支持以下参数：

* `--documents`：指定输入文档路径（支持本地或S3路径）。
* `--taggers`：指定要运行的`tagger`（可多个）。
* `--experiment`：指定输出目录的名称（可选）。
* `--processes`：指定并行处理的进程数（默认为1）。

示例命令：

```Plain Text
dolma tag \
    --documents "s3://path/to/documents/*.json.gz" \
    --taggers cld2_en_doc_v2 char_length_v1 \
    --processes 4
```
```python
dolma tag \
    --documents "wikipedia/v0/documents/*" \  # 输入文档路径（支持通配符*匹配）
    --experiment exp \                       # 输出目录命名为attributes/exp/
    --taggers random_number_v1 \             # 为每个文档生成随机数（用于数据集划分）
              cld2_en_paragraph_with_doc_score_v2 \  # 用cld2检测段落语言并计算英语占比
              ft_lang_id_en_paragraph_with_doc_score_v2 \  # 用fastText检测段落语言（与cld2互为验证）
              char_length_with_paragraphs_v1 \  # 统计文档和段落的字符数
              whitespace_tokenizer_with_paragraphs_v1 \  # 统计按空格分词的单词数
    --processes 16   # 启动16个并行进程（建议在96核CPU上使用）
```
```python
dolma tag \
    --documents "/mnt/wikipedia/*/documents/*" \  # 匹配所有文件夹
    --experiment wikipedia_exp \
    --taggers random_number_v1 \
              cld2_en_paragraph_with_doc_score_v2 \
              char_length_with_paragraphs_v1 \
              whitespace_tokenizer_with_paragraphs_v1 \
    --processes 24
```
```python
dolma tag \
    --documents "/mnt/wikipedia/*/documents/*" \
    --experiment wikipedia_exp \
    --taggers random_number_v1 \
              cld2_en_paragraph_with_doc_score_v2 \
              ft_lang_id_en_paragraph_with_doc_score_v2 \  # 补充 fastText 验证
              char_length_with_paragraphs_v1 \
              whitespace_tokenizer_with_paragraphs_v1 \
              pii_regex_v2 \                               # 隐私过滤
              jigsaw_hatespeech_document_v2 \              # 有害内容过滤
              gopher_v1 \                                  # 近似重复检测
              olmo_pretokenizer_v1                         # 精确 Token 计数
    --processes 24
```


\# Dolma Tagger 组合分析与优化建议



\## 当前 Tagger 组合功能分析



\### 1. \`random\_number\_v1\`

\- \*\*作用\*\*：为文档分配随机数，用于后续数据集划分（训练/验证/测试集）

\- \*\*评估\*\*：

  - 基础必要功能，但仅涉及数据分割，不涉及内容清洗



\### 2. \`cld2\_en\_paragraph\_with\_doc\_score\_v2\`

\- \*\*作用\*\*：检测段落语言（英语）并计算文档级英语分数

\- \*\*评估\*\*：

  - \*\*优势\*\*：有效过滤非英语内容（适合英语大模型训练）

  - \*\*局限\*\*：仅依赖 CLD2，可能误判小语种或混合语言文本（需补充 fastText 或 cld3 验证）



\### 3. \`char\_length\_with\_paragraphs\_v1\`

\- \*\*作用\*\*：统计文档和段落的字符长度

\- \*\*评估\*\*：

  - \*\*优势\*\*：过滤过短（垃圾文本）或过长（未分段）内容

  - \*\*局限\*\*：无法检测低质量文本的语义问题（如重复、无意义内容）



\### 4. \`whitespace\_tokenizer\_with\_paragraphs\_v1\`

\- \*\*作用\*\*：统计空格分隔的单词数（Token 数）

\- \*\*评估\*\*：

  - \*\*优势\*\*：近似估计文本长度，辅助过滤异常文档

  - \*\*局限\*\*：与真实 Tokenizer（如 BPE）结果存在差异，可能需 \`olmo\_pretokenizer\_v1\` 更精确计数



\## 缺失功能与补充建议



| 需求                | 推荐补充的 Tagger                  | 原因                                                                 |

|---------------------|-----------------------------------|----------------------------------------------------------------------|

| 高质量语言检测      | \`ft\_lang\_id\_en\_\*\` 或 \`cld3\_en\_\*\` | 提高非英语内容检测精度                                               |

| 内容安全过滤        | \`jigsaw\_hatespeech\_\*\`, \`jigsaw\_nsfw\_\*\` | 去除有害/NSFW 内容                                                  |

| PII 隐私过滤        | \`pii\_regex\_v2\`, \`pii\_presidio\_v1\` | 去除个人信息（如邮箱、电话）                                         |

| 代码/版权处理       | \`code\_copyright\_comments\_v1\`, \`code\_secrets\_v1\` | 若含代码需处理版权和密钥                                           |

| 近似重复检测        | \`gopher\_v1\`                      | 基于 DeepMind 规则去重                                              |

| 精确 Token 计数     | \`olmo\_pretokenizer\_v1\`           | 更接近大模型的真实 Token 化                                         |



\## 优化后的完整 Tagger 组合建议



\`\`\`bash

dolma tag \\

    --documents "/mnt/wikipedia/\*/documents/\*" \\

    --experiment wikipedia\_exp \\

    --taggers random\_number\_v1 \\

              cld2\_en\_paragraph\_with\_doc\_score\_v2 \\

              ft\_lang\_id\_en\_paragraph\_with\_doc\_score\_v2 \\

              char\_length\_with\_paragraphs\_v1 \\

              whitespace\_tokenizer\_with\_paragraphs\_v1 \\

              pii\_regex\_v2 \\

              jigsaw\_hatespeech\_document\_v2 \\

              gopher\_v1 \\

              olmo\_pretokenizer\_v1 \\

    --processes 24