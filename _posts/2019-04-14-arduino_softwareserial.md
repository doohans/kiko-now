---
layout: post
title: "Arduino SoftwareSerial on ArduinoMEGA2560"
tags: [arduino, mega, softwareserial]
comments: true
published : true
---

ArduinoMEGA2560에서 SoftwareSerial 사용하기

### 왜??

ArduinoMEGA2560은 하드웨어 시리얼 포트가 3개가 있으므로 다른 arduino 보드들과는 다르게 시리얼 포트의 여유가 있다. 그래서 SoftwareSerail 포트를 쓸 일이 거의 없을 것이다. 나는 thing 제작에는 arduino pro mini 를 사용한다. 그래서 시리얼 통신이 필요하면 SoftwareSerial이 필수적이다. 
그래도 ArduinoMEGA2560가 핀수도 많고 여러모로 편한 점이 있기에 개발시에 디버깅 보드로 사용하고 있다. 추후 arduino pro mini와 코드 호환을 위해 시리얼 통신은 SoftwareSerial을 사용한다.

### MEGA에서 SoftwareSerial 제약

매번 까먹는 제약사항이다. 아래 나열된 핀 이외에 핀을 사용하면 SoftwareSerial이 동작하지 않는다.

>Not all pins on the Mega and Mega 2560 support change interrupts, so only the following can be used for RX: 10, 11, 12, 13, 14, 15, 50, 51, 52, 53, A8 (62), A9 (63), A10 (64), A11 (65), A12 (66), A13 (67), A14 (68), A15 (69). 

https://www.arduino.cc/en/Reference/softwareSerial
