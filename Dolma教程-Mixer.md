

1. **用途​**​：合并多源数据（需为 Dolma 格式），应用过滤规则和文本替换，生成统一输出。
2. ​**​输入要求​**​：文档需包含标记的重复段落（通过前置 Tagger 处理）。
3. ​**​关键操作​**​：

* 合并属性（如 `exp`、`bff_duplicate_paragraph_spans`）。
* 基于 JSONPath 过滤文档（`include`/`exclude`）。
* 替换指定文本片段（`span_replacement`）。

---
### ​**​配置示例（JSON/YAML）​**​
#### 配置文件 `wikipedia-mixer.json`：
```Plain Text
{
  "streams": [
    {
      "name": "getting-started",
      "documents": ["/mnt/wikipedia/v0/documents/*.gz"],
      "output": {
        "path": "/mnt/wikipedia/example0/documents",
        "max_size_in_bytes": 1000000000
      },
      "attributes": ["exp", "bff_duplicate_paragraph_spans"],
      "filter": {
        "include": ["$.attributes[?(@.exp__whitespace_tokenizer_with_paragraphs_v1__document[0][2] < 100000)]"],
        "exclude": [
          "$.attributes[?(@.exp__whitespace_tokenizer_with_paragraphs_v1__document[0][2] < 50)]",
          "$.attributes[?(@.exp__ft_lang_id_en_paragraph_with_doc_score_v2__doc_en[0][2] <= 0.5)]",
          "$.attributes[?(@.bff_duplicate_paragraph_spans[0][2] >= 1.0)]"
        ]
      },
      "span_replacement": [
        {
          "span": "$.attributes.exp__cld2_en_paragraph_with_doc_score_v2__not_en",
          "min_score": 0.1,
          "replacement": ""
        }
      ]
    }
  ],
  "processes": 1
}
```
#### 参数说明：
* ​**​****streams\[\].filter****​**​：
* `include`：保留文档的条件（如 token 数 < 10万）。
* `exclude`：排除文档的条件（如低质量段落、非英文内容、高重复段落）。\* ​**​****span\_replacement****​**​：替换非英文段落为空白（`min_score`控制阈值）。

---
### ​**​运行命令​**​
```Plain Text
dolma -c dolma_config/wikipedia-mixer.json mix --processes 16
```
* `--processes 16`：覆盖配置中的进程数，加速处理

---
### ​**​输出结果​**​
* 目录结构：`wikipedia/example0/documents/`
* 文件内容：通过过滤的文档，属性合并后按 `max_size_in_bytes` 分块存储。

---
### ​**​常见问题​**​
1. ​**​输入路径格式​**​：支持本地路径（`/mnt/...`）或 S3（`s3://bucket/path`）。
2. ​**​调试技巧​**​：添加 `"dryrun": true` 预览配置，不实际执行
3. 。
4. ​**​性能优化​**​：增加 `processes` 或调整 `max_size_in_bytes` 控制文件大小。

---
### ​**​参考资源​**​
* Dolma 项目地址：allenai/dolma

配置模板：wikipedia-mixer.yaml

