---
layout: post
title: "구형 라즈베리파이 Motion 적용하기"
tags:
  - 라즈베리파이
  - Motion
  - PI
  - PICAM
comments: true  
---

성능이 후달리는 구형 라즈베리파이를 이용한 스트리밍 서버 구현하기를 해본다.

서랍속에 굴러다니는 구형 라즈베리파이(Raspberry Pi Model B Rev 2)를 다시 꺼냈다. PI CAM을 이용한 단순한 스트리밍 서버로만 쓸 것이다. 구글링을 해보니 관련 자료가 한가득 나온다. 워낙 내 라즈베리파이가 구형이라 글도 모두 하나 같이 오래된 자료다. 포스트의 대다수가 pi cam을 스트리밍 서버로 쓸때 motion-mmal 설정에 대해 나온다. 하지만 최신 Motion 을 설치하니 이 과정이 없이도 성공적으로 스트리밍이 된다.

## 설치순서

> 1.Raspbian Stretch Lite 설치
> 
> 2.Motion 최신버전 설치

### 1.Raspbian Stretch Lite 설치

워낙 구형 라즈베리이기에 최소 설치만 진행하려고 가장 용량이 작은 배포판을 선정하였다. 

https://www.raspberrypi.org/downloads/raspbian/

가이드대로 설치하고 raspi-config에가서 wifi 설정을 해준다. 

![2019-01-20 09-20-23](https://user-images.githubusercontent.com/19382541/51433745-ae81f200-1c94-11e9-9350-67baf3784d27.png)

5번 항목인 interfacing Options 항목에서 P1 Camera와 P2 SSH 를 활성화 한다. 

![2019-01-20 09-21-34](https://user-images.githubusercontent.com/19382541/51433752-d1aca180-1c94-11e9-9d6c-394c6aa719be.png)

### 2.Motion 최신버전 설치

아래 출처의 사이트의 방법으로 하니 매우 쉽게 설정이 되었다.

[출처]
https://www.bouvet.no/bouvet-deler/utbrudd/building-a-motion-activated-security-camera-with-the-raspberry-pi-zero

pi 전용으로 제작된 motion을 다운로드한다.

```
wget https://github.com/Motion-Project/motion/releases/download/release-4.2.1/pi_stretch_motion_4.2.1-1_armhf.deb
```

gdebi로 데비안 패키지로 된 motion을 설치한다. gdebi가 없으면 sudo apt-get install gdebi-core 명령으로 설치하고 진행한다.

```
sudo gdebi pi_stretch_motion_4.2.1-1_armhf.deb
```

/etc/motion/motion.conf 모션 설정은 아래 항목은 필수로 설정하고 그 외는 각자 취향것 설정.

```
1. mmalcam_name vc.ril.camera
2. stream_localhost off 
3. webcontrol_localhost off
```


/etc/rc.local 파일 exit 0 위에 motion 을 추가하여 재기동시 자동 실행 되도록 한다.

```
motion 
```

라즈베리파이 재기동 후 접속할 단말 브라우저에서 http://라즈베리파이ip:8080 하면 webcontrol 화면이 나오고 정상적이라면 하나의 카메라가 활성화 된게 보인다.

![2019-01-20 09-24-29](https://user-images.githubusercontent.com/19382541/51433807-97dc9a80-1c96-11e9-9356-5220912d05aa.png)
