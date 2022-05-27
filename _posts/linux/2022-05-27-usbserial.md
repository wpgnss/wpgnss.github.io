---
layout: post
current: post
cover:  assets/blog/linux-minecraft.png
navigation: True
title: ubuntu 22.04 에서 USB Serial(/dev/ttyUSB) 인식되지 않는 문제
tags: [linux]
class: post-template
subclass: 'post'
author: daniel
---

ubuntu 22.04 에서 usb serial 장치를 사용하려고 연결해보았습니다.

이전에 사용하던 배포판(20.04) 에서는 연결하고 dialout, tty 그룹정도만 추가해주면 됬는데.. ubuntu 22.04에서는 잘되지 않았다..

~~~bash
$ sudo usermod -a -G dialout $USER
$ sudo usermod -a -G tty $USER
~~~


1. lsusb 로 USB Serial 인식 확인됨
    - Silicon Labs CP210x UART Bridge

![](assets/images/2022-05-27-usbserial/Untitled.png)

1. syslog 확인해보니 brltty에 의해 USB driver가 정상적으로 access 되지 않고 있음
    - sudo dmesg

![](assets/images/2022-05-27-usbserial/Untitled%201.png)

1. brltty
    - ubuntu 22.04 에서 기본 소프트웨어로 추가된 점자 디스플레이 관련 소프트웨어

![](assets/images/2022-05-27-usbserial/Untitled%202.png)

1. brltty 삭제
- 필요하지 않을 것으로 예상되어 brltty 삭제
- sudo apt autoremove brltty
- 혹시 brltty를 사용한다면 rule을 수정해야함 ([https://unix.stackexchange.com/questions/670636/unable-to-use-usb-dongle-based-on-usb-serial-converter-chip](https://unix.stackexchange.com/questions/670636/unable-to-use-usb-dongle-based-on-usb-serial-converter-chip))

![](assets/images/2022-05-27-usbserial/Untitled%203.png)

1. USB Serial 인식

![](assets/images/2022-05-27-usbserial/Untitled%204.png)

![](assets/images/2022-05-27-usbserial/Untitled%205.png)