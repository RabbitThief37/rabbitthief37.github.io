---
layout: post
title:  "ESP32-CAM 구매시 주의할 점"
description: "불량 많은 ESP32-CAM, 교환 오래 걸리는 디바이스 마트"
author: RabbitThief
date: 2020-11-30 10:56:00 +0900
tags: arduino esp32cam devicemart 
category: arduino
comments: false
---	



# 구매시 주의 할 점

회사에서 황당한? 프로젝트를 진행하면서 구세주처럼 사용한 것이 ESP32-CAM이다.  BT 와 WiFi를 같이 지원하는 저렴한 CAM이라는 장점이 매우 강력하다.  

주문~

### 

![/assets/article_images/2020-11-30/Untitled.png](/assets/article_images/2020-11-30/Untitled.png)

연결하는 법, CAM Server 설정하는 법, 샘플 코드 설명 등등 검색만 해도 한글을 비롯해서 차고 넘치게 나오니 이 부분은 삭제.

디바이스마트에서 마침 행사를 해서 개당 8,000원 구매를 했는데, 설명 페이지에 있는대로 해 봤는데...어라?? 이미지 캡쳐가 안된다.  

구글님에게 많은 질문을 하면서, 먼가 설정이 잘되었나 싶어서 이짓 저짓을 다 해 보았다.  물론 시간도 하염없이 보내고...쳇...

어쩔 수 없이 아래와 같이 문의를 했지만,

```
**올려 놓은 동영상과 서술한 내용대로 실행을 했으나**

WiFi connected
Starting web server on port: '80'
Starting stream server on port: '81'
Camera Ready! Use '[http://192.168.0.166](http://192.168.0.166/)' to connect
[E][camera.c:1344] esp_camera_fb_get(): Failed to get the frame on time!
Camera capture failed
ets Jun 8 2016 00:22:57

에러가 출력되서 이미지가 찍히거나 스트리밍되지 않습니다.
검색을 통하여 다양한 방법으로 시도를 해 보았으나, 역시 위의 에러가 반복적으로 발생합니다.
자세히 보니 홈페이지 상품 이미지에는 AI-Thinker 라는 문구가 있으나, 
배송된 제품에는 그 부분이 없습니다. 혹 비슷하지만 다른 제품이 배송된 것인지요?
아니면 해결책이 있는지 알고 싶습니다
```

돌아온 답변은 이미 상품페이지에 있는 안내 그대로 Copy & Paste 

```
문의주신 내용에 대해 번거로우시겠지만  
Features 영상에 나온 모듈은 다른 버전인 MODEL_WROVER_KIT 입니다.  
해당 방법으로 에러 발생 시  아래와 같이 코드 변경후 진행부탁드립니다. 

// Select camera model
//#define CAMERA_MODEL_WROVER_KIT
//#define CAMERA_MODEL_ESP_EYE
//#define CAMERA_MODEL_M5STACK_PSRAM
//#define CAMERA_MODEL_M5STACK_WIDE
#define CAMERA_MODEL_AI_THINKER
로 변경후,

툴 -> 보드 -> ESP32 Arduino -> AI Thinker ESP32-CAM 으로 변경 하여 진행 탁드리며, 

재 시도 에도 동일한경우 번거로우시겠지만, 주문번호 + 구매자(수령자) 연락처 
재 문의주시면 추가 확인 도움드리겠습니다.
```

머지???라고 하면서 해당 장치를 잘 넣어 두고 다음꺼를 했는데... 잘 된다... 

곧 불량인 것을 하필 처음으로 잡은 운도 지지리도 없는 경우라고 할까나.

교환을 바로 신청하고 ( 자기네들이 체크해서 불량아니면 왕복 택배비 내야한다는 전화 안내를 꼭!!! 한다 ), 거의 일주일을 기다려서 새제품을 받았다.  개느림...

그 뒤로 프로젝트가 진행되면서 더 필요하게 되어서 3번의 추가 구매가 있었다.

![/assets/article_images/2020-11-30/Untitled1.png](/assets/article_images/2020-11-30/Untitled1.png)

이 구매 중에서도 교환 신청을 안한 적이 딱 1번이다.  나머지는 모두 1개이상 교환신청. 헐...

마지막 주문에서는 3개 중에서 2개의 카메라 모듈이 흑백으로 나와서 교환을 진행했다.  물론 교환까지 받는데 기본 2주정도 걸린것 같다. 

역시 중국제라서 그런지 QC가 엉망인거 같다.  **구매해서 먼가 해 보실 분이 있다면 절대 1개만 사는 우를 범하지 마시길.  최소 3개이상은 구매한다고 생각해라.**  불량이 얼마가 나올지 모른다.

마지막으로 팁을 하나 추가하자면, **테스트로 사용하는 CameraWebServer 라는 샘플을 WiFi SSID 와 Password를 사내에 맞게 수정한 후 저장해 놓자.**  이거 확인할 때마다 다시 설정하는거 은근히 귀찮다. (2.4Ghz만 지원)