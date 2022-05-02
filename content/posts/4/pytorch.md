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

## 構築方法

上記の環境で[前回の記事](https://kaedefuto.github.io/kaede_blog/posts/1/conda/)と同様に**conda**を使って[PyTorch](https://pytorch.org/)をインストールしましたが，エラーが出たので記事にします．

[前回の記事](https://kaedefuto.github.io/kaede_blog/posts/1/conda/)通りに環境を作成し，GPUが使えるかどうかを確認します．

```
import torch  
print(torch.cuda.is_available())
```

無事**True**と表示されたのでGPUが使えると思いきや，実際に学習させると以下のエラーが出たため学習ができませんでした．

```
__init__.py:104: UserWarning: 
NVIDIA RTX A6000 with CUDA capability sm_86 is not compatible with the current PyTorch installation.
The current PyTorch install supports CUDA capabilities sm_37 sm_50 sm_60 sm_70.
If you want to use the NVIDIA RTX A6000 GPU with PyTorch, please check the instructions at https://pytorch.org/get-started/locally/

  warnings.warn(incompatible_device_warn.format(device_name, capability, " ".join(arch_list), device_name))

RuntimeError: CUDA error: no kernel image is available for execution on the device
```

エラー内容はPyTorchに対応するCUDAのバージョンがあっていないとのことなので，PyTorchのバージョンを色々変更しましたが同じエラーが出てしまいました．
また，CUDAのバージョンも変更しましたがダメでした．

## 解決策

わからないのでエラー内容を調べてみると，**conda**だとこのGPU(NVIDIA RTX A6000)は対応していないことがわかりました([参考](https://github.com/pytorch/pytorch/issues/52288))．また，**conda**ではなく**pip**だといけそうなので試してみました([参考](https://teratail.com/questions/358588))．

今回は[Torchtext](https://pytorch.org/text/stable/index.html#)の0.9.0を使うため，[TorchtextのバージョンにあったPytorch](https://pytorch.org/get-started/previous-versions/)をインストールしました([参考](https://github.com/pytorch/text#installation))．

```
pip install torch==1.8.0+cu111 torchvision==0.9.0+cu111 torchaudio==0.8.0 -f https://download.pytorch.org/whl/torch_stable.html
```

GPUが使えるかどうかを確認します．

```                       
import torch
print(torch.cuda.is_available())
print(torch.version.cuda)
print(torch.__version__)
print(torch.cuda.current_device())
print(torch.cuda.get_arch_list())

#True
#11.1
#1.8.0+cu111
#0
#['sm_37', 'sm_50', 'sm_60', 'sm_70', 'sm_75', 'sm_80', 'sm_86']
```

これでエラーも消え，学習することができました．
最新のPyTorchをインストールした後に，バージョンが対応していないTorchtextをインストールすると，PyTorchのバージョンが変わるのでご注意ください．(最新のTorchtextが色々変わりすぎて使いずらい...)

