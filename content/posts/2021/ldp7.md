---
title: "炼丹杂记 -- 低版本 PyTorch 中 pack_padded_sequence 缺少 enforce_sorted 参数问题解决方案"
date: 2021-06-26T10:35:58+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pytorch", "rnn", "python"]
draft: false
---

## 背景

近期用到了一台新的堡垒机，上面的驱动环境比较老只能用 1.0.0 版本的 PyTorch。  

调试了代码以后发现大体上没有遇到什么问题，唯一就是涉及到 GRU 的 `torch.nn.utils.rnn.pack_padded_sequence` 以及 `torch.nn.utils.rnn.pad_packed_sequence` 两个接口。  

在 1.1.0 版本以后前者新增了 `enforce_sorted` 参数，将其设为 False 则接口会自动对输入的序列按长度排序。  

而老版本的 PyTorch 中没有这一参数，因此需要自行实现排序以及运算过后的解排序，略为麻烦但也能够实现，特此记录一下具体方法。  

## 高版本 PyTorch 实现

`sentence_embedding` 为句子嵌入，`sentence_length` 为表示 batch 中各句子长度的标量数组，`self.gru` 为实例化好的 bi-GRU。  

有 `enforce_sorted` 参数时，如下即可实现所需的运算。  

```
sentence_pack = torch.nn.utils.rnn.pack_padded_sequence(sentence_embedding, sentence_length, batch_first=True, enforce_sorted=False)
sentence_gru,_ = self.gru(sentence_pack)
sentence_unpacked = torch.nn.utils.rnn.pad_packed_sequence(sentence_gru, batch_first=True)[0]
```

## 低版本 PyTorch 实现

没有 `enforce_sorted` 参数的情况下，需要先对输入的特征进行人为排序，并在运算过后进行解排序。  

```
# 排序
_, idx_sort_sen = torch.sort(torch.Tensor(sentence_length.type(torch.FloatTensor)), dim=0, descending=True)
_, idx_unsort_sen = torch.sort(idx_sort_sen, dim=0)
idx_sort_sen = idx_sort_sen.cuda()
idx_unsort_sen = idx_unsort_sen.cuda()
sentence_embedding = sentence_embedding.index_select(0, idx_sort_sen)
sentence_length = list(sentence_length[idx_sort_sen])

# 运算
sentence_pack = torch.nn.utils.rnn.pack_padded_sequence(sentence_embedding, sentence_length, batch_first=True)
sentence_gru,_ = self.gru(sentence_pack)
sentence_unpacked = torch.nn.utils.rnn.pad_packed_sequence(sentence_gru, batch_first=True)[0]

# 解排序
sentence_unpacked = sentence_unpacked.index_select(0, idx_unsort_sen)
```
