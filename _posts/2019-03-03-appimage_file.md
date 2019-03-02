---
layout: post
title: "Appimage 파일에 관해"
tags: [appimage,packaging]
comments: true
published : true
---

appimage 파일의 내용을 보는 방법 

3d 프린터 슬라이스 프로그램인 Cura가 appimage 파일로 배포가 된다. 우분투에서 데스크탑 바로가기를 등록하려니 아이콘 파일이 없어서 잘 되지 않는다. appimage 파일 자체에는 아이콘 파일이 포한된 듯 하여 추출하여 사용하기로 하였다.

### appimage 추출 방법

appimage 패키징된 버전에 따라 두가지로 나뉜다.

#### type 1

loop 마운트 시켜서 내용을 볼 수 있다.
```
sudo mount -o loop ./Ultimaker_Cura-3.6.0.AppImage /mnt/cura
```

#### type 2

최신 Appimage로 패키징하면 type 2로 되는 것 같다. 실행하려는 appimage 파일 뒤에 파라미터로 `--appimage-help` 라고 입력하면 가능한 명령들이 나오고 `--appimage-extract` 명령을 이용하여 appimage 파일의 내용을 추출 할 수 있다.

```
./SomeAppImage.Appimage --appimage-help
AppImage options:

--appimage-extract              Extract content from embedded filesystem image
--appimage-help                 Print this help
--appimage-mount                Mount embedded filesystem image and
                                print mount point, then wait for kill with Ctrl-C
--appimage-offset               Print byte offset to start of
                                embedded filesystem image
--appimage-signature            Print digital signature embedded in AppImage
--appimage-updateinfo[rmation]  Print update info embedded in AppImage
--appimage-version              Print version of AppImageKit
```
