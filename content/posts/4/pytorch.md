---
title: "pipでPytorch環境作成"
date: 2022-04-30T10:08:14+09:00
draft: false
tags: [Pytorch,pip]
categories: [Pytorch]
summary: "pipでGPUが使えるPytorch環境を構築"
author: "kaedefuto"
---

# PyTorch環境作成

## 環境
- Ubuntu:20.04
- GPU:NVIDIA RTX A6000
- CUDA Version: 11.6

上記の環境で[前回の記事](https://kaedefuto.github.io/kaede_blog/posts/1/conda/)と同様に**conda**を使って[PyTorch](https://pytorch.org/)をインストールしました．

GPUが使えるかどうかを確認します．

```
import torch  
print(torch.cuda.is_available())
```

**True**と表示されたのGPUが使えると思いきや，実際に学習させると以下のエラーが出て，学習ができませんでした．

```
__init__.py:104: UserWarning: 
NVIDIA RTX A6000 with CUDA capability sm_86 is not compatible with the current PyTorch installation.
The current PyTorch install supports CUDA capabilities sm_37 sm_50 sm_60 sm_70.
If you want to use the NVIDIA RTX A6000 GPU with PyTorch, please check the instructions at https://pytorch.org/get-started/locally/

  warnings.warn(incompatible_device_warn.format(device_name, capability, " ".join(arch_list), device_name))

RuntimeError: CUDA error: no kernel image is available for execution on the device
```

PyTorchのバージョンやCUDAのバージョンがあってなさそうなので，PyTorchのバージョンとCUDAのバージョンを変更しましたが常に同じエラーが出てしまいました．
わからないのでエラーを調べてみると，**conda**だとこのGPU（NVIDIA RTX A6000）は対応していないそうです．**pip**だと行けるとのことだったので試してみました．

今回はTorchtextの0.9.0を使うため，TorchtextのバージョンにあったPytorchをインストールしました．

```
pip install torch==1.8.0+cu111 torchvision==0.9.0+cu111 torchaudio==0.8.0 -f https://download.pytorch.org/whl/torch_stable.html
```

これでエラーも消え，学習することができました．

