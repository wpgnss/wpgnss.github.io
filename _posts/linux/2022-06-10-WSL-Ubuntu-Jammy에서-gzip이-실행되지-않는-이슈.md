---
layout: post
current: post
cover:  assets/blog/linux-minecraft.png
navigation: True
title: WSL Ubuntu Jammy에서 gzip이 실행되지 않는 이슈
tags: [linux]
class: post-template
subclass: 'post'
author: daniel
---

gzip을 사용할 때 아래와 같은 메시지를 출력하며 에러 발생

```bash
/bin/sh: 1: gzip: Exec format error
```

gzip을 직접 사용해도 에러가 나고, kernel build 등에서 gzip이 사용될 때에도 에러가 발생 함

```bash
CHK     include/generated/compile.h
GZIP    kernel/config_data.gz
/bin/sh: 1: gzip: Exec format error
make[1]: *** [kernel/Makefile:139: kernel/config_data.gz] Error 126
make[1]: *** Deleting file 'kernel/config_data.gz'
make: *** [Makefile:1822: kernel] Error 2
```

gzip을 재설치해도 똑같은 현상이 발생함

### 관련 이슈

[https://github.com/microsoft/WSL/issues/8219](https://github.com/microsoft/WSL/issues/8219)

### 원인

위 이슈를 보았을 때, gzip [1.10-4ubuntu1](http://archive.ubuntu.com/ubuntu/pool/main/g/gzip/gzip_1.10-4ubuntu1_amd64.deb) 은 정상동작 하는데, [1.10-4ubuntu3](http://archive.ubuntu.com/ubuntu/pool/main/g/gzip/gzip_1.10-4ubuntu3_amd64.deb) 버전에서 해당 증상이 발생하고 있음

우분투가 아닌 WSL1에서 ELF Parser에서 발생한 버그이며 자세한 내용은 아래와 같다. (**[hzqmwne](https://github.com/hzqmwne))**

[https://github.com/microsoft/WSL/issues/8219#issuecomment-1094123281](https://github.com/microsoft/WSL/issues/8219#issuecomment-1094123281)

### 해결방법

**[dreamlayers](https://github.com/dreamlayers)** 의 해결방법

```bash
echo -en '\x10' | sudo dd of=/usr/bin/gzip count=1 bs=1 conv=notrunc seek=$((0x189))
```