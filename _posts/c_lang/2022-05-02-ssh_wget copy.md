---
layout: post
current: post
cover:  assets/blog/c-cpp-minecraft.png
navigation: True
title: c lang test
date: 2022-05-02 00:00:00
tags: [linux]
class: post-template
subclass: 'post'
author: daniel
---


**RSSH(Reverse SSH)** 로 연결된 리눅스 디바이스에 **wget** 명령을 사용하여 파일을 업로드/다운로드 하려고 한다.

작업하려는 디바이스가 하나라면 연결된 RSSH 세션으로 접속하여 wget 명령을 사용하고 기다리면 된다.

하지만 작업하려는 디바이스가 여러대라면 모든 디바이스를 순차적으로 작업할 수는 없다.

이럴 땐 아래와 같이 **-b** 옵션을 사용하면 wget을 **background** 에서 동작할 수 있고, SSH 세션이 종료되더라도 업로드/다운로드를 수행한다.

~~~bash
wget -b http://file-server.com/large-file.iso
~~~


[man wget](https://linux.die.net/man/1/wget)을 보면 -b 옵션의 설명은 아래와 같다.

~~~bash
-b, --background
Go to background immediately after startup. If no output file is specified via the -o, output is redirected to wget-log.
~~~

