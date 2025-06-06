### 官网：
[官网链接](http://www.aias.top/)

### 下载模型
- 查看最新下载链接请查看 1_sdks/README.md

### 模型使用方法：
- 1. 用模型的名字搜索代码，找到模型的加载位置
- 2. 然后更新模型路径（代码里默认加载路径是：项目/models 文件夹）
- 3. 具体模型加载方法
- http://aias.top/AIAS/guides/load_model.html


### 句向量SDK【支持100种语言】
句向量是指将语句映射至固定维度的实数向量。
将不定长的句子用定长的向量表示，为NLP下游任务提供服务。

- 支持下面100种语言：          
![img](https://aias-home.oss-cn-beijing.aliyuncs.com/AIAS/nlp_sdks/languages_100.jpeg)
 
- 句向量         
![img](https://aias-home.oss-cn-beijing.aliyuncs.com/AIAS/nlp_sdks/Universal-Sentence-Encoder.png)

-  

句向量应用：
- 语义搜索，通过句向量相似性，检索语料库中与query最匹配的文本
- 文本聚类，文本转为定长向量，通过聚类模型可无监督聚集相似文本
- 文本分类，表示成句向量，直接用简单分类器即训练文本分类器

### SDK功能：
-  句向量提取
-  相似度（余弦）计算
-  max_seq_length: 128（subword切词，如果是英文句子，上限平均大约60个单词）

#### 运行例子 - SentenceEncoderExample
运行成功后，命令行应该看到下面的信息:
```text
...
# 测试语句：
# 英文一组
[INFO ] - input Sentence1: This model generates embeddings for input sentence
[INFO ] - input Sentence2: This model generates embeddings

# 中文一组
[INFO ] - input Sentence3: 今天天气不错
[INFO ] - input Sentence4: 今天风和日丽

# 向量维度：
[INFO ] - Vector dimensions: 768

# 英文 - 生成向量：
[INFO ] - Sentence1 embeddings: [0.10717804, 0.0023716218, ..., -0.087652676, 0.5144994]
[INFO ] - Sentence2 embeddings: [0.06960095, 0.09246655, ..., -0.06324193, 0.2669841]

#计算英文相似度：
[INFO ] - 英文 Similarity: 0.84808713

# 中文 - 生成向量：
[INFO ] - Sentence1 embeddings: [0.19896796, 0.46568888,..., 0.09489663, 0.19511698]
[INFO ] - Sentence2 embeddings: [0.1639189, 0.43350196, ..., -0.025053274, -0.121924624]

#计算中文相似度：
#由于使用了sentencepiece切词器，中文切词更准确，比15种语言的模型（只切成字，没有考虑词）精度更好。
[INFO ] - 中文 Similarity: 0.67201

```

### 开源算法
#### 1. sdk使用的开源算法
- [sentence-transformers](https://github.com/UKPLab/sentence-transformers)
- [预训练模型](https://www.sbert.net/docs/pretrained_models.html)
- [安装](https://www.sbert.net/docs/installation.html)


#### 2. 模型如何导出 ?
- [how_to_convert_your_model_to_torchscript](http://docs.djl.ai/docs/pytorch/how_to_convert_your_model_to_torchscript.html)

- 导出CPU模型（pytorch 模型特殊，CPU&GPU模型不通用。所以CPU，GPU需要分别导出）
- device = torch.device("cpu")
- device = torch.device("gpu")
- export_model_100.py
```text
from sentence_transformers import SentenceTransformer
import torch

# model = SentenceTransformer('stsb-distilbert-base', device='cpu')
model = SentenceTransformer('paraphrase-xlm-r-multilingual-v1', device='cpu')
model.eval()
batch_size=1
max_seq_length=128
device = torch.device("cpu")
model.to(device)
input_ids = torch.zeros(batch_size, max_seq_length, dtype=torch.long).to(device)
input_type_ids = torch.zeros(batch_size, max_seq_length, dtype=torch.long).to(device)
input_mask = torch.zeros(batch_size, max_seq_length, dtype=torch.long).to(device)
# input_features = (input_ids, input_type_ids, input_mask)
input_features = {'input_ids': input_ids, 'attention_mask': input_mask}

# traced_model = torch.jit.trace(model, example_inputs=input_features)
traced_model = torch.jit.trace(model, example_inputs=input_features,strict=False)
traced_model.save("models/paraphrase-xlm-r-multilingual-v1/paraphrase-xlm-r-multilingual-v1.pt")
```



### 其它帮助信息
http://aias.top/guides.html


### Git地址：   
[Github链接](https://github.com/mymagicpower/AIAS)    
[Gitee链接](https://gitee.com/mymagicpower/AIAS)   



#### 帮助文档：
- http://aias.top/guides.html
- 1.性能优化常见问题:
- http://aias.top/AIAS/guides/performance.html
- 2.引擎配置（包括CPU，GPU在线自动加载，及本地配置）:
- http://aias.top/AIAS/guides/engine_config.html
- 3.模型加载方式（在线自动加载，及本地配置）:
- http://aias.top/AIAS/guides/load_model.html
- 4.Windows环境常见问题:
- http://aias.top/AIAS/guides/windows.html