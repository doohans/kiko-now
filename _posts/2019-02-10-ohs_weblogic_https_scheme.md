---
layout: post
title: "OHS & Weblogic https scheme 문제"
tags: [https,ssl,ohs,weblogic]
comments: true
published : true
---

ohs & weblogic 구성 시 ohs만 ssl 설치를 하게 되면 weblogic 단에서 request.getScheme() 시에 https가 아닌 http로 반환하는 현상이 있다. was 상에서는 ssl 설정을 하지 않았기에 정상일수도 있겠지만 개발하는 개발자와의 환경도 불일치하고 운영 서비스를 https로 하게되어 브라우저에서 https로 유입이 되면 의례 java code에서는 request.getScheme()시에 https라고 예상하게 된다.

![image](https://user-images.githubusercontent.com/19382541/52530648-dfad8980-2d4b-11e9-806e-decd25f51c66.png){: .center-image}


# web server 헤더 설정

아파치에서는 [X-Forwarded-Proto 값을 헤더에 설정하여 처리하는 방법](https://stackoverflow.com/questions/25911469/request-getscheme-is-returning-http-instead-of-returning-https-in-java)이 쉽게 검색이 된다. ohs & weblogic 조합에서는 이 헤더가 동작하지 않는다. 별도 오라클에서 설정한 방법으로 ssl을 전달해줘야 한다. ohs에서는 mod_wl 파일을 수정하여 [WLProxySSLPassThrough On](https://docs.oracle.com/cd/E13222_01/wls/docs81/plugins/plugin_params.html) 으로 설정한다.

# was 설정

ohs에서 설정한 WLProxySSL을 weblogic에서 처리하기 위해 플러그인 활성화를 해야 한다. 도메인명->configuration탭->Web Applications탭 으로 이동 후 Weblogic Plugin Enabled 를 체크 한다. weblogic plugin의 자세한 설명은 [여기](http://www.ateam-oracle.com/wls-plugin-enabled/)를 참고한다.




