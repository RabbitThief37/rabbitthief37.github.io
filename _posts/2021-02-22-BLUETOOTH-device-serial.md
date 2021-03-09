---
layout: post
title:  "ESP32-CAM과 BLE 데이터 연결 실패기"
description: "ESP32-CAM 이미지를 다른 회사의 BT BLE로 시도하다가 실패하고 ESP32로 연결한 썰"
author: RabbitThief
date: 2021-02-22 00:00:00 +0900
tags: bluetooth esp32cam ble bgx13p hc06 
category: Knowledge
comments: false
---	



# BLUETOOTH

이미지를 촬영하여 BT로 전송을 해야 하는 작업을 했다.  회사에서는 주력으로 사용하는 BT 모듈은 Silicon Labs BGX13P ( [https://www.silabs.com/wireless/bluetooth/bgx13-wireless-xpress-modules](https://www.silabs.com/wireless/bluetooth/bgx13-wireless-xpress-modules) ) 여서 Main board에 장착하고 ESP32-CAM과 BLE 통신을 시도해 보았다.

- SLEXP8027A ( BGX13P Wireless Xpress Starter Kit )

![/assets/images/2021/0222/Untitled.png](/assets/images/2021/0222/Untitled.png)

**연결은 되지만 데이터가 안 간다 ???  머시기 ???**

![/assets/images/2021/0222/Untitled1.png](/assets/images/2021/0222/Untitled1.png)

안드로이드 스맛폰에 다른 사람들이 추천하는 확인용 앱인 nRF Connect( [https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp&hl=ko&gl=US](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp&hl=ko&gl=US) )를 사용하면, BGX13P 와 ESP32-CAM 둘 다 정상적으로 동작한다.  하지만 둘 사이는 절대 안된다.  물론 필자의 BLE 실력이 미천하여 무엇인가 빠진 부분이 있을 수가 있다라는 가능성도 있지만...쳇

BGX13P BLE는 Binary Data 전송에 대해서 보증을 못한다는 HMD Korea의 FA님 말씀도 있고 해서 BT 2.0 Spec인 BT Serial로 변경해서 도전을 해 보았다.  벗드... BGX13P는 지원 안한다.  앵??? 이봐... ESP32가 지원하는데 너가 안된다고??? 그렇단다.  BT Serial은 따로 칩이 있다고.

머...별 수 있나.  검색을 해 보니 가장 많이 아두이노랑 연결해서 사용하는 제품인 HC-06을 2개 구매해서 ESP32와 연결을 시도해 보았다. 

![/assets/images/2021/0222/Untitled2.png](/assets/images/2021/0222/Untitled2.png)

HC-06 지들끼는 아무런 문제 없이 BT Serial로 잘 동작을 한다.  하지만 ESP32랑은 안되더라.  와... 무선의 세계란~~~

결국은 깔끔하게 GG 치고 ESP32-CAM에서 CAM 때고 연결했더니, 한방에 통신 성공

Bluetooth가 예상했던 만큼의 만능은 아니라는거. 

왠지 이전에 HDMI 호환성 개고생하던 것을 구경하는 느낌이랄까나.