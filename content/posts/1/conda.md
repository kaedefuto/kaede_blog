---
title: "PyTorch環境作成"
date: 2021-05-04T18:38:26+09:00
draft: false
tags: [PyTorch]
categories: [PyTorch]
summary: "AnacondaでGPUが使えるPyTorch環境を構築"
author: "kaedefuto"
---

# PyTorch環境作成

BERTを実装する際のAnacondaでGPUが使えるPyTorch環境を構築します．

## 環境
- Ubuntu:18.04
- GPU:Quadro RTX 8000
- CUDA Version: 11.2

## 構築方法

まず**conda**で仮想環境を作成します．

```
$ conda　create -n pytorch python=3.7
```

作成した環境に**activate**します．

```
$ conda　activate pytorch
```

次にパッケージを[ここから](https://pytorch.org/) **conda**でインストールします．
CUDAのバージョンが11.2なので，今回はこちらをインストールします．

```
$ conda install pytorch==1.8.0 torchvision==0.9.0 torchaudio==0.8.0 cudatoolkit=11.1 -c pytorch -c conda-forge
```

最後にGPUが使えるかどうかを確認します．

```
import torch  
print(torch.cuda.is_available())
```
**True**と表示されたらOKです．
