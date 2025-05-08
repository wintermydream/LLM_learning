测试模型-预训练参数设置

```python
run_name: OLMo-1B
seed: 6198
dry_run: false

wandb:
  name: ${run_name}
  project: olmo-small

# 模型配置
model:
  d_model: 2048
  n_heads: 16
  n_layers: 16
  mlp_ratio: 8
  weight_tying: true
  alibi: false
  rope: true
  flash_attention: true  # not available on AMD
  attention_dropout: 0.0
  attention_layer_norm: false
  multi_query_attention: false
  include_bias: false
  block_type: sequential
  layer_norm_type: default
  layer_norm_with_affine: false
  bias_for_layer_norm: false
  attention_layer_norm_with_affine: false
  activation_type: swiglu
  residual_dropout: 0.0
  embedding_dropout: 0.0
  max_sequence_length: 2048
  vocab_size: 50280
  embedding_size: 50304
  eos_token_id: 50279  # 注意, 需要与tokenizer中的词表一致
  pad_token_id: 1  # 注意, 需要与tokenizer中的词表一致
  init_device: meta
  init_fn: mitchell

compile: ##null  # causes instability on AMD GPUs
  backend: inductor

# 优化器配置
optimizer:
  name: adamw
  learning_rate: 4.0e-4
  weight_decay: 0.1
  betas:
  - 0.9
  - 0.95
  metrics_log_interval: 10

scheduler:
  name: cosine_with_warmup
  t_warmup: 2000
  alpha_f: 0.1

# tokenizer配置
tokenizer:
  identifier: tokenizers/allenai_eleuther-ai-gpt-neox-20b-pii-special.json
  truncate_direction: right

# checkpoint保存配置
#save_folder: ${path.choose:${oc.env:SCRATCH_DIR,no_exist}/checkpoints,/results}/${oc.env:SLURM_JOB_ID,${run_name}}
save_folder: /home/olmo/checkpoints/${run_name}  # 或 /home/olmo/results/${run_name}
save_overwrite: true
# Sharded checkpoints (best for restarts)
save_interval: 200
save_num_checkpoints_to_keep: 9
# Unsharded checkpoints (for final storage)
save_interval_unsharded: 10000
save_num_unsharded_checkpoints_to_keep: -1

# checkpoint加载路径
load_path: null
  #'/mnt/geogpt-gpfs/llm-course/home/yly/OLMo/checkpoints/OLMo-1B/step1000/'

# 训练步数及batchsize
max_duration: 739_328  # 3.1T tokens
global_train_batch_size: 32
device_train_microbatch_size: 2

precision: amp_bf16

fsdp:
  wrapping_strategy: null
  precision: mixed
  # sharding_strategy: SHARD_GRAD_OP

max_grad_norm: 1.0
max_grad_norm_ratio: null

speed_monitor:
  window_size: 20

# 训练时长
time_limit: 864000

# 验证集地址
eval_interval: ${save_interval}
eval_subset_num_batches: -1
device_eval_batch_size: ${device_train_microbatch_size}
evaluators:
  # lump all the small datasets together (we still get separate metrics).
  - label: v3-small-ppl-validation
    data:
      num_workers: 0
      drop_last: true
      datasets:
        v3-small-c4_en-validation:
          - https://olmo-data.org/eval-data/perplexity/v3_small_gptneox20b/c4_en/val/part-0-00000.npy
        v3-small-dolma_books-validation:
          - https://olmo-data.org/eval-data/perplexity/v3_small_gptneox20b/dolma_books/val/part-0-00000.npy
        v3-small-dolma_common-crawl-validation:
          - https://olmo-data.org/eval-data/perplexity/v3_small_gptneox20b/dolma_common-crawl/val/part-0-00000.npy
        v3-small-dolma_pes2o-validation:
          - https://olmo-data.org/eval-data/perplexity/v3_small_gptneox20b/dolma_pes2o/val/part-0-00000.npy
        v3-small-dolma_reddit-validation:
          - https://olmo-data.org/eval-data/perplexity/v3_small_gptneox20b/dolma_reddit/val/part-0-00000.npy
        v3-small-dolma_stack-validation:
          - https://olmo-data.org/eval-data/perplexity/v3_small_gptneox20b/dolma_stack/val/part-0-00000.npy


  - label: v2-small-ppl-validation
    data:
      num_workers: 0
      drop_last: true
      datasets:
        v2-small-4chan-validation:
        - https://olmo-data.org/eval-data/perplexity/v2_small_gptneox20b/4chan/val.npy
        v2-small-c4_100_domains-validation:
        - https://olmo-data.org/eval-data/perplexity/v2_small_gptneox20b/c4_100_domains/val.npy
        v2-small-c4_en-validation:
        - https://olmo-data.org/eval-data/perplexity/v2_small_gptneox20b/c4_en/val.npy
        v2-small-gab-validation:
        - https://olmo-data.org/eval-data/perplexity/v2_small_gptneox20b/gab/val.npy

  # 声明对验证集所要执行的测试任务
  - label: piqa
    type: downstream

  - label: hellaswag
    type: downstream

  - label: winogrande
    type: downstream

  - label: openbook_qa
    type: downstream

  # - label: boolq  # requires implemention of the pmi_dc matrix
    # type: downstream

  - label: sciq
    type: downstream

  - label: arc_easy
    type: downstream

  # - label: arc_challenge  # requires implemention of the pmi_dc matrix
  #   type: downstream

  - label: copa
    type: downstream

  - label: rte
    type: downstream

  - label: commitment_bank
    type: downstream

  - label: mrpc
    type: downstream

  - label: sst2
    type: downstream

# 训练数据位置
data:
  pad_direction: right
  num_workers: 0
  drop_last: true
  pin_memory: true
  prefetch_factor: 16
  persistent_workers: true
  timeout: 0
  paths:
    - /mnt/zzb/peixunban/hzq/olmo_datasets/preprocessed/olmo-mix/v1_5/gpt-neox-20b-pii-special/part-000-00000.npy

```