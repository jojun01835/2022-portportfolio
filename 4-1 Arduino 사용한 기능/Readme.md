# 2022-portportfolio
<br>

**Android 사용한 기술**

* HC-06<br>
* DHT 11 <br>
* LCD(Liquid Crystal Display)<br>

<br>
<br>

## Arduino

<br>
<br>

**HC-06 Module**

<br>
<br>

* HC-06는 무선으로 시리얼 통신을 동작하게 해주는 블루투스 모듈입니다. HC-06 모듈은 아래 그림과 같이 사각형, 원형 등 다양한 종류가 있습니다. <br>
  아래의 그림에서 노란색을 표시한 초록색 보드가 HC-06이고, 여러 제조사에서 HC-06 보드와 6Pin, 4Pin 등으로 인터페이스 회로를 구성 후 HC-06 Module을 만들고 판매를 합니다.<br>
  <br>
  
 <div align=center>
  
  ![1](https://user-images.githubusercontent.com/73435598/206860092-8b057147-4708-4350-a9bd-9add7a351421.png)

</div>

* 여러가지 블루투스 모듈<br><br>

* 주요 사양<br><br>

* 동작전압 : 3.6~6V<br>
* 소모전류 : 40mA<br>
* 통신방식 : Serial (UART)<br>
* 제어방식 : AT-Command<br>
* 블루투스 사양 : Bluetooth V2.0 Protocol Standard<br>
* 파워 레벨 : Class2(+6dBm) - 통신 거리 10m<br>
* RF 대역 : 2.4GHz ~ 2.48GHz, ISM Band<br>
* 수신감도 : -80dBm<br><br>

**핀 아웃**<br><br>

* HC-06은 보통 아래의 6Pin 또는 4Pin으로 구성되어 있습니다. 일반적인 경우 VCC, GND, TXD, RXD의 4핀만 사용합니다.<br>

* KEY(or EN) : HC-06 모드 선택핀<br>
* High : AT Command mode<br>
* LOW or NC : Normal mode<br>
* RXD : 3.3V 레벨 시리얼 수신 (Default 9600bps)<br>
* TXD : 3.3V 레벨 시리얼 송신 (Default 9600bps)<br>
* GND : 그라운드 연결<br>
* VCC : +3.3 ~ 6V 전원 연결<br>
* Status : 보드 상태 출력핀으로, 보통 상태 LED와 연결되어 있음<br>
<br>
<br>

## DHT-11 온습도센서
<br>
<br>

* 스펙<br>

* 습도 측정 범위 : 20-90% RH<br>

* 습도 오차 범위 : ±5% RH<br>

* 측정 온도 범위 : 0-50 °C<br>

* 온도 오차 범위 : ±2% °C<br>

* 동작 전압 : 5V<br>

* 소비 전력 : 저전력<br>

<br>
<br>

<div align=center>
  
  ![2](https://user-images.githubusercontent.com/73435598/206868122-fdefe1da-6cab-469d-8761-baaba068eb0c.png)

</div>

<br>
<br>

**온습도 센서 기본원리**<br><br>

* DHT11은 두 전극 사이의 전기 저항을 측정하여 수증기를 감지합니다. <br>

* 습도 감지 구성 요소는 전극이 표면에 적용된 수분 보유 기판입니다. <br>

* 수증기가 기판에 흡수되면 이온이 기판에 의해 방출되어 전극 사이의 전도성이 증가합니다 <br>

* 두 전극 사이의 저항 변화는 상대 습도에 비례합니다. <br>

* 상대 습도가 높을수록 전극 사이의 저항이 감소하고 상대적 습도가 낮 으면 전극 사이의 저항이 증가합니다 <br>

* 온도는 표면에 설치된  NTC 온도 센서 (써미스터)를 사용하여 온도에 따른 물질의 저항치 변화값으로 온도를 측정 합니다 <br>

<br>
<br>

**제어**<br><br>

<div align=center>

  ![3](https://user-images.githubusercontent.com/73435598/206868259-ea272c79-c854-4d7b-900a-a07da4cb7365.png)

</div>

<br>
<br>

* DHT11센서는 Signal 핀의 레벨을 통해 통신을 위와 같이 수행합니다<br>

* 초기 18ms 동안 LOW로 유지하다 HIGH로 전환하여 DHT에 요청을 하게되고<br>

* 20us~40us이후 DHT에서 50us간 LOW로 80us간 HIGH로 전환하여 진행에 대한 반환을 하게 됩니다<br>

* 이후 온습도 데이터를 전송하며 비트전송시 50us LOW후 26~28us HIGH는 0bit로 50us LOW후 70us HIGH는 1bit로 간주하여 데이터를 전송합니다<br>

* 데이터의 끝에서는 50us LOW후 계속 HIGH상태를 유지합니다<br>

<br>
<br>

## LCD(Liquid Crystal Display)<br><br>

* LCD는 액정 디스플레이 또는 액정 표시장치를 의미하며 뒷면에 빛을 내는 백라이트(backlight)를 배치하고 앞면에 액정을 두어 전기신호에 따라 빛을 차단하거나 통과시키는 방식으로   문자나 숫자 등을 표시하는 장치입니다. <br><br>
* 아두이노 시뮬레이터에서 제공하는 LCD를 1602 LCD라고도 하는데, 가로로 16개의 문자를 출력할 수 있고 세로로 2줄을 표현할 수 있는 LCD라는 의미입니다.<br><br>

* LCD 16x2 I2C 핀 4핀으로 구성<br>
* Gnd<br>
* Vcc(5V)<br>
* 데이터핀(A4)<br>
* 클럭핀(A5)<br>
<br>
* LCD 16x2 I2C 주소 확인법
  <br>
 
  [LCD 주소 확인법](https://playground.arduino.cc/Main/I2cScanner/)
 
 <br>
 <br>
 
 ## referance
 
 




