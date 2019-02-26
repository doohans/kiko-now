---
layout: post
title: "Jenkins Gitlab Webhook"
tags:
  - Gitlab
  - Webhook
  - Jenkins 
comments: true  
---

gitlab webhook 시 브랜치 필터링!

정규표현식 너무 어렵다! 그냥 특정 문자열(ex. deploy, release 등)이 포함된 브랜치가 배포 브랜치에 Merge Request 승인이 되면 자동으로 서버 배포 후 재기동하고 싶다. 

```
^\S*(?i)(release)\S*$
```

브랜치 이름에 release라는(대소문자를 구분하지 않음) 문구가 들어가는 걸 체크함.

* ^ : 문자열 시작
* \S : 공백 제외 다른 문자
* \* : 앞의 문자가 존재하지 않을 수 도 있고 다수 존재 할 수 도 있음
* (?i) : 대소문자를 구분하지 않음
* (release) : 패턴 문자열
* $ : 문자열 끝
