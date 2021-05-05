---
title: "ufwの設定"
date: 2021-05-05T10:19:07+09:00
draft: false
tags: [ufw,Ubuntu]
categories: [Ubuntu]
summary: "ufwコマンドでファイアーフォールの設定"
author: "kaedefuto"
---


# ufwコマンドでファイアーフォールの設定

ufwコマンドでファイアーウォールを設定します．  
ufwはファイアーウォールを設定するコマンドです．

## 環境
- Ubuntu18.04

## 各種コマンド

ufwの状態とルールの表示
```
$ sudo ufw status numberd
```
ufwの有効化
```
$ sudo ufw enable
```
ufwの無効化
```
$ sudo ufw disable
```
ルールのリセット
```
$ sudo ufw reset
```
ルールの追加  
fromで指定したアドレスからtoで指定したアドレスかつポート番号への通信を受信(制限)します．     
アドレスを指定しない場合は```Anywhere```とし，ポート番号を指定しない場合は```any port```とします．  
```
$ sudo ufw allow from "ネットワークアドレス" to "ホストアドレス" port "ポート番号""
$ sudo ufw limit from "ネットワークアドレス" to "ホストアドレス" port "ポート番号""
```
ルールの削除
```
$ sudo ufw delete "番号"
```
ファイアーウォールの最読込
```
$ sudo ufw reload
```

## 設定方法
まずufwの状態を表示します．
```
$ sudo ufw staus

#状態：非アクティブ
```
次にファイアウォールを有効化します．
```
$ sudo ufw enable
```
次にルールを追加します．  
ルールは順番を考慮する必要があリ，追加したルールは一番最後の位置に追加されます．  
位置を指定して追加する場合は```insert```を使用します．
```
$ sudo ufw insert 1 allow from xxx.xxx.xxx.0/24 to xxx.xxx.xxx.ooo port 22
$ sudo ufw insert 2 limit from Anywhere to any port 22
$ sudo ufw status numberd

#    To                         Action      From
     --                         ------      ----
[ 1] xxx.xxx.xxx.ooo 22         ALLOW IN    xxx.xxx.xxx.0/24          
[ 2] 22                         LIMIT IN    Anywhere  
```
最後にルールを最読込して終了です．
```
$ sudo ufw reload
```
