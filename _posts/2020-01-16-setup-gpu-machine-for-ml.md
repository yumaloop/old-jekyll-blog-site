---
layout: post
title: Setting up a GPU machine for Machine Learning
categories:
    - Tips
tags:
    - hoge
    - foo
---


In this note, I describe how to install NVIDIA GPU and set up CUDA/cuDNN on Ubuntu 16.04LTS machine that has been clean booted. Also, I write down some linux commands used in debugging, since knowing your machine in detail would lead to resolving some errors related to the machine environment. This article could be updated from time to time.
 

> Example: My Ubuntu GPU machine (2020/01/10)
> - OS :
> 	- [Ubuntu 16.04.6 LTS](http://releases.ubuntu.com/16.04/) (GNU/Linux 4.4.0-145-generic x86_64)
> - RAM(16GB) : 
> 	- Memory: [Kingston 8GB 288-Pin DDR4 SDRAM DDR4 2133 (PC4 17000)](https://www.newegg.com/kingston-8gb-288-pin-ddr4-sdram/p/N82E16820242069) x2
> - ROM(250GB): 
>   - SSD: [Samsung SSD 750 EVO 250GB](https://www.samsung.com/us/computing/memory-storage/solid-state-drives/mz-750250bw-mz-750250bw/) (/dev/sda)
>   - HDD: [Seagate Barracuda ST2000DM001 Desktop SATA](https://www.disctech.com/Seagate-Barracuda-2000GB-SATA-Hard-Drive-ST2000DM001) (/dev/sdb)
> - CPU(x8) :
>   -  [Intel Core i7-6700 CPU @ 3.40GHz](https://ark.intel.com/content/www/us/en/ark/products/88196/intel-core-i7-6700-processor-8m-cache-up-to-4-00-ghz.html)
> - GPU(x1) : 
>   -  NVIDIA [Geforce GTX 1080](https://www.nvidia.com/en-us/geforce/products/10series/geforce-gtx-1080/)
>   	-  NVIDIA CUDA : 10.0.130 (/usr/local/cuda-10.0/)
>   	-  NVIDIA cuDNN : 7.4.2.24 (/usr/lib/x86_64-linux-gnu/libcudnn.so.7.4.2)
>   		-  Python3 : 3.6.9 (/usr/bin/python3.6)
>   		-  Python2 : 2.7.12 (/usr/bin/python)
>   		-  tensorflow 1.13.1 ($HOME/.local/lib/python3.6/site-packages)
>   		-  tensorflow-gpu 1.13.1 ($HOME/.local/lib/python3.6/site-packages)
>   		-  keras 2.2.4 ($HOME/.local/lib/python3.6/site-packages)
>   		-  pytorch 1.2.0 ($HOME/.local/lib/python3.6/site-packages)

<br>

> Table of contents
> - [オペレーティングシステム（OS）](#-------------os-)
> 	- [OSを確認したい](#os------)
> 	- [Linuxディストリビューションを確認したい](#linux-----------------)
> 	- [Linuxカーネルを確認したい](#linux----------)
> - [ストレージ（ROM）](#------rom-)
> 	- [HDDの状態を確認したい](#hdd---------)
> 	- [ファイル数を確認したい](#-----------)
> 	- [ファイルのディスク使用量を確認したい](#------------------)
> - [メモリ（RAM）](#----ram-)
> 	- [メモリデバイスを確認したい](#-------------)
> 	- [メモリの空き容量を確認したい](#--------------)
> - [CPU](#cpu)
> 	- [CPUデバイスを確認したい](#cpu----------)
> - [GPU](#gpu)
> 	- [GPUデバイスの確認](#gpu-------)
> - [NVIDIAドライバとCUDA/cuDNNの導入](#nvidia-----cuda-cudnn---)
> 	- [NVIDIAドライバのインストール](#nvidia-----------)
> - [CUDAのインストール](#cuda-------)
> 	- [cuDNNのインストール](#cudnn-------)
> 	- [PATHチェック](#path----)
> - [ディスプレイ](#------)
> 	- [X11ディスプレイマネージャ(DM)を確認](#x11------------dm----)

<br>
<br>


## オペレーティングシステム（OS）

##### OSを確認したい

unameコマンドを使う

「OS名,ホスト名,OSリリース,OSバージョン,マシンアーキテクチャ,CPUタイプ,プラットフォーム,OS名」が順に表示される．

```bash
$ uname -a
Linux XXXX 4.4.0-145-generic #171-Ubuntu SMP Tue Mar 26 12:43:40 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux

```





##### Linuxディストリビューションを確認したい

/etc/issueファイルをみる

```bash
$ cat /etc/issue
Ubuntu 16.04.6 LTS \n \l
```



/etc/lsb-releaseをみる

```bash
$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04 DISTRIB_CODENAME=xenial 
DISTRIB_DESCRIPTION="Ubuntu 16.04.6 LTS"
```



/etc/os-releaseをみる

```bash
$ cat /etc/os-release
NAME=“Ubuntu”
VERSION=“16.04.6 LTS (Xenial Xerus)” ID=ubuntu ID_LIKE=debian PRETTY_NAME=“Ubuntu 16.04.6 LTS” VERSION_ID=“16.04" HOME_URL=“http://www.ubuntu.com/” SUPPORT_URL=“http://help.ubuntu.com/” BUG_REPORT_URL=“http://bugs.launchpad.net/ubuntu/” VERSION_CODENAME=xenial UBUNTU_CODENAME=xenial
```



##### Linuxカーネルを確認したい

/proc/versionをみる

```bash
$ cat /proc/version
Linux version 4.4.0-159-generic (buildd@lgw01-amd64-042) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.10) ) #187-Ubuntu SMP Thu Aug 1 16:28:06 UTC 2019
```



 

## ストレージ（ROM）

ストレージデバイス（HDD, SSD）とファイルシステムまわりについて．

##### HDDの状態を確認したい

df -hコマンドを使う

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            7.8G     0  7.8G   0% /dev
tmpfs           1.6G   46M  1.6G   3% /run
/dev/sda1       214G  165G   39G  81% /
tmpfs           7.9G  208K  7.9G   1% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           7.9G     0  7.9G   0% /sys/fs/cgroup
/dev/loop3      384K  384K     0 100% /snap/patchelf/93
/dev/loop1      384K  384K     0 100% /snap/patchelf/87
none            7.9G  2.5M  7.9G   1% /tmp/guest-qyuodw
tmpfs           1.6G   64K  1.6G   1% /run/user/998
/dev/loop4       90M   90M     0 100% /snap/core/8213
/dev/loop0       90M   90M     0 100% /snap/core/8268
tmpfs           1.6G     0  1.6G   0% /run/user/1001
```



##### ファイル数を確認したい

wcコマンドを使う

カレントディレクトリ直下にあるファイル数を表示する

```bash
$ du -hsc *
689M	Research
4.0K	build
106M	dataset
4.0K	docker
9.3M	gym
50M	kaggle
2.6M	latent.gif
2.0G	opencv
122G	workspace
4.0K	ダウンロード
4.0K	テンプレート
4.0K	デスクトップ
4.0K	ドキュメント
4.0K	ビデオ
4.0K	ピクチャ
4.0K	ミュージック
4.0K	公開
125G	合計
```





##### ファイルのディスク使用量を確認したい

df -hコマンドを使う

カレントディレクトリ直下にあるファイルおよびディレクトリのディスク使用量とその合計を表示する

```bash
$ du -hsc *
689M	Research
4.0K	build
106M	dataset
4.0K	docker
9.3M	gym
50M	kaggle
2.6M	latent.gif
2.0G	opencv
122G	workspace
4.0K	ダウンロード
4.0K	テンプレート
4.0K	デスクトップ
4.0K	ドキュメント
4.0K	ビデオ
4.0K	ピクチャ
4.0K	ミュージック
4.0K	公開
```



## メモリ（RAM）

##### メモリデバイスを確認したい

/proc/meminfoをみる

メモリの詳細情報が表示される

```bash
$ cat /proc/meminfo
MemTotal: 16377200 kB
MemFree: 3077848 kB
MemAvailable: 15767804 kB
Buffers: 363052 kB
Cached: 12274992 kB
SwapCached: 66936 kB
Active: 8048088 kB
Inactive: 4689560 kB
Active(anon): 25860 kB
Inactive(anon): 86584 kB
...
HugePages_Total: 0
HugePages_Free: 0
HugePages_Rsvd: 0
HugePages_Surp: 0
Hugepagesize: 2048 kB
DirectMap4k: 1907316 kB
DirectMap2M: 14815232 kB
DirectMap1G: 0 kB
```



##### メモリの空き容量を確認したい

freeコマンドを使う

```bash
$ free
              total        used        free      shared  buff/cache   available
Mem:       16377148     2470228      314496       17140    13592424    13460232
Swap:      16720892      431568    16289324
```



vmstatコマンドを使う

```bash
$ vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0 431584 267696 944212 12638044    0    2   389    15    0    0  6  2 91  0  0
```



topコマンドを使う

```bash
$ top
top - 15:55:05 up 64 days, 23:12,  5 users,  load average: 1.00, 1.04, 1.07
Tasks: 232 total, 2 running, 230 sleeping, 0 stopped, 0 zombie
%Cpu(s): 9.1 us, 3.5 sy, 0.0 ni, 86.9 id, 0.5 wa, 0.0 hi, 0.0 si, 0.0 st
KiB Mem : 16377148 total,   271964 free,  2527528 used, 13577656 buff/cache
KiB Swap: 16720892 total, 16289228 free,   431664 used. 13403420 avail Mem 

...
```





## CPU

##### CPUデバイスを確認したい

/proc/cpuinfoをみる

CPUのコアごとに詳細情報が表示される

```

$ cat /proc/cpuinfo
processor : 0
vendor_id : GenuineIntel
cpu family : 6
model : 94
model name : Intel(R) Core(TM) i7-6700 CPU @ 3.40GHz
stepping : 3
microcode : 0xc6
cpu MHz : 800.062
cache size : 8192 KB
physical id : 0
siblings : 8
...
processor : 1
vendor_id : GenuineIntel
cpu family : 6
model : 94
model name : Intel(R) Core(TM) i7-6700 CPU @ 3.40GHz
...
```





## GPU

##### GPUデバイスの確認

lswsコマンドを使う

```bash
$ sudo lshw -C display 
  *-display               
       詳細: VGA compatible controller
       製品: GP104 [GeForce GTX 1080]
       ベンダー: NVIDIA Corporation
       物理ID: 0
       バス情報: pci@0000:01:00.0
       バージョン: a1
       幅: 64 bits
       クロック: 33MHz
       性能: pm msi pciexpress vga_controller bus_master cap_list rom
       設定: driver=nvidia latency=0
       リソース: irq:317 メモリー:de000000-deffffff メモリー:c0000000-cfffffff メモリー:d0000000-d1ffffff IOポート:e000(サイズ=128) メモリー:df000000-df07ffff
```



lspciコマンドを使う

Linuxに搭載されているPCIバスの情報を表示する．

```bash
$ lspci | grep -i nvidia
01:00.0 VGA compatible controller: NVIDIA Corporation GP104 [GeForce GTX 1080] (rev a1)
01:00.1 Audio device: NVIDIA Corporation GP104 High Definition Audio Controller (rev a1)
```





## NVIDIAドライバとCUDA/cuDNNの導入

##### NVIDIAドライバのインストール


1.下記リンクから，自分のGPUにあうドライバを検索してダウンロードする．

https://www.nvidia.co.jp/Download/index.aspx?lang=jp

![img](https://lh4.googleusercontent.com/m4MAKmekqihk5pqTUHLojPX6uzAZoaGoj2d9EazHOSlVxNuradgm_lF-7piupC_j1dBf-PffJ2e1SaDxf215GddSYx_XdaUAyUkucebB5SSyN9ZrGUl7qeqFe0aUxnnBqQxYYeK7)

 

たとえば，GPU「[NVIDIA](http://d.hatena.ne.jp/keyword/NVIDIA) [GeForce](http://d.hatena.ne.jp/keyword/GeForce) 1080」に対応したドライバは以下のようになる．

![img](https://lh3.googleusercontent.com/e951y4mwYumub525KR2Um_Cqbb6NxYIpnyThgMyIcClu7nvwHUzF5hGK5l3_4k0lPxf9T9uF33u-0t0cTJeovZFNsjXJkBBzCM1B61i4Kk8_xIBSsTWT0oUexsNDx5_QRFrIDIIj)．

2. 新しく[GPU](http://d.hatena.ne.jp/keyword/GPU)ドライバ（[NVIDIA](http://d.hatena.ne.jp/keyword/NVIDIA)ドライバ）をインストールする前に，既にインストールされている[GPU](http://d.hatena.ne.jp/keyword/GPU)ドライバを確認する．



aptにNVIDIAドライバを提供しているxorg-edgersレポジトリを追加する．



aptでNVIDIAドライバ「nvidia-396」をインストールして，マシンを再起動．



## CUDAのインストール

（注意）CUDA・cuDNN・tensorFlow-gpuのバージョンを合わせる必要がある．

1. CUDAの公式ドキュメントをよく読む．

   CUDA Toolkit Documentation https://docs.nvidia.com/cuda/index.html

2. 下記リンクから，NVIDIAドライバに対応するCUDAのバージョンを確認する

   CUDA Toolkit Documentation > Release Notes > 1. CUDA Toolkit Major Components > CUDA Driver https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html

![img](https://lh3.googleusercontent.com/s8gpeKo-VZlCtUROQc6YG1L3ZQ9aJlggnswp_mAAxRFua_Sh0ILtYsa-K4uUYWrjbfgz21EIVJwSrt-StpbGhvpPrxUytW-Xjy146HAo7pmo-SyPT6cP5-YPSFWRHHASmR9xyFon)

3. 下記リンクから，tensorflow-[gpu](http://d.hatena.ne.jp/keyword/gpu)に対応するcuDNN/CUDAのバージョンを確認する

   TensorFlow (Linux) - テスト済みのビルド設定

   https://www.tensorflow.org/install/source#linux

![img](https://lh6.googleusercontent.com/LrKW-yUDWLZi5-iq1oz0BB_3dpt3hin3yLEyo8V1QtJzV958tWDaYr5RDwJsOniMxotiNqZsCcYa4-kqs3SdSLd7xzD9ABON4b2MT9kIHPbbnL3lP9GcgZpFRTWjVThXPuQMQNUP)

CUDA・cuDNN・tensorFlow-gpuのバージョン確認を終えた．



今回は，以下で環境構築をする．

> - [Python](http://d.hatena.ne.jp/keyword/Python) 3.6.9
> - tensorflow-[gpu](http://d.hatena.ne.jp/keyword/gpu) 1.13.1
> - CUDA 10.0
> - cuDNN 7.4




4. 下記リンクから，自分の環境にあった「CUDA Toolkitパッケージ」を確認し，マシンへダウンロードする．

   CUDA Toolkit Archive https://developer.nvidia.com/cuda-toolkit-archive

![img](https://lh6.googleusercontent.com/6Wg51qcEzuCZ326X81F_48j0pU20MC4qKa0CU847Vw612vZQNUP302zFYWCJ9CA5uKABt6UU7UTLj8M6Kr9ndYKNzT-amTFv3hiWGDakR5gpjFlA3lppzcU-dORL0ojBI6qs-Pv5)

今回は，CUDA10.0で，マシンの環境として，以下を選択．

> - Operating System: Linux
> - Architecture: x86_64
> - Distribution: Ubuntu
> - Version: 16.04
> - Installer Type: deb [network]



![img](https://lh6.googleusercontent.com/qkrUa2T8IaJ7IflSrZ6XCyDvHqmoP868EnpDlqOoSoYiMcse_8xteV7gEUsmAS_2GCr7GWUI_rClrw7E6i3RuKUKghWm-L5fT8vST6r9CnCHBtxwIS5LvB18ZfMY1Aet173jSWzC)

 

（注意）https://developer.nvidia.com/cuda-downloadsは，最新バージョンのダウンロードリンクなので，ここから安易にCUDAをダウンロードしてはいけない．特に，tensorflow-[gpu](http://d.hatena.ne.jp/keyword/gpu)は，最新のCUDA Toolkitに対応していないので注意する．CUDAとTensorflow-[gpu](http://d.hatena.ne.jp/keyword/gpu)のバージョンがあっていないと，たとえばImportError: libcublas.so.10.0が発生する．

対応するCUDA Toolkit（CUDA 10.0）の.[deb](http://d.hatena.ne.jp/keyword/deb)ファイル(network)は「cuda-repo-ubuntu1604_10.0.130-1_[amd64](http://d.hatena.ne.jp/keyword/amd64).[deb](http://d.hatena.ne.jp/keyword/deb)」となる． この.[deb](http://d.hatena.ne.jp/keyword/deb)ファイルをwgetコマンドを使って，マシンへダウンロードする．



5. ダウンロードしたCUDA Toolkitパッケージ(.deb)を，マシンへインストールする

dpkgコマンドでCUDA Toolkitパッケージ(.deb)をcudaパッケージとして保存します．さらに，aptコマンドでcudaパッケージをインストールします． 注意：公式に書かれているsudo apt-get install cudaを実行すると自動的に最新版のCUDAがインストールされる．



これでCUDA Toolkit（CUDA 10.0）のインストールは完了．

次に，[環境変数](http://d.hatena.ne.jp/keyword/%B4%C4%B6%AD%CA%D1%BF%F4)（PATH）を設定する．

##### cuDNNのインストール

##### PATHチェック


## ディスプレイ

##### X11ディスプレイマネージャ(DM)を確認

/etc/X11/default-display-managerをみる



