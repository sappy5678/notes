---
title: NLP Transformers 模型部署
categories:
  - DL
  - NLP
  - Deploy
tags:
  - DL
  - NLP
  - Transformers
  - AI
  - Deploy
date: 2021-11-15T15:33:45.604Z
---

# 起因
畢業之後，在實驗室協助學長部屬要給聯合報使用的自然語言處理系統，由於速度過慢，因此首要任務便是提高模型的處理速度

# 常見的模型加速方法
1. 使用更好的硬體(GPU、CPU(AVX512)、FPGA)
2. 使用專門的運行環境 (ONNX)
3. 調整推理方式(降低無用的計算)

# 我的解法
使用 ONNX 及 支援 AVX512 的 CPU，並不使用 batch 計算，降低 batch 計算時出現的無用 padding

# 結果
將模型的 throughput 提高 9 倍， latency 降低至原本的 1/6


# 參考資料
* [Scaling up BERT-like model Inference on modern CPU - Part 1](https://huggingface.co/blog/bert-cpu-scaling-part-1)
* [Scaling up BERT-like model Inference on modern CPU - Part 2](https://huggingface.co/blog/bert-cpu-scaling-part-2)
* [How We Scaled Bert To Serve 1+ Billion Daily Requests on CPUs - Roblox Blog](https://blog.roblox.com/2020/05/scaled-bert-serve-1-billion-daily-requests-cpus/)
* [How to 10x throughput when serving Hugging Face models without a GPU – Comet](https://www.comet.ml/site/how-to-10x-throughput-when-serving-hugging-face-models-without-a-gpu/)
