# 2022-portportfolio
<br>

**Android 회로도,코드**


<br>
<br>

## Arduino

<br>
<br>

**UVC LED**

* UVC LED 회로도

<br>

<div align=center>

<img whith="30%" height="30%" src="https://user-images.githubusercontent.com/73435598/201585003-3809f414-b1eb-4be5-acfb-26a6d8a2b020.PNG"/><br>

</div>

<br>
<br>

* UVC LED 코드<br>

```c
#define LED1  9 //LED핀 번호
#define LED2 10 

void setup(){
 pinMode(LED2, OUTPUT);
 pinMode(LED1, OUTPUT);
}
void loop(){
if(data == 'a'){  
      digitalWrite(LED2, HIGH);  //LED 켜기
      digitalWrite(LED1, HIGH);  //LED 켜기
  }
  
  else if(data == 'b'){  
      digitalWrite(LED2, LOW);  //LED 끄기
      digitalWrite(LED1, LOW);  //LED 끄기
      
  }
}
```

<br>
<br>

## LCD 회로도 <br><br>

<div align=center>

![4](https://user-images.githubusercontent.com/73435598/206869249-a0a0705f-c184-4dd9-852f-b2a3d9cfe70d.png)

</div>

<br>
<br>

**LCD 코드**

```c
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27, 16, 2);
void setup()
{
Serial.begin(9600);  //하드웨어 시리얼 통신 준비
 lcd.init(); 		// initialize the lcd 
  lcd.backlight();
  lcd.setCursor(2,0);
  delay(5000);
  lcd.clear();
}
void loop()
{
float h = dht.readHumidity();
  float t = dht.readTemperature();

  if(isnan(h) || isnan(t)){
    Serial.println(F("Failed to read from DHT sensor!"));
    return;
  }

  Serial.print((int)t); Serial.print("*C ");
  Serial.print((int)h); Serial.println("%");

  //delay(1000);

  lcd.setCursor(0,0); // LCD Cursor 원점
  lcd.print("TEMP: "); // LCD에 "temp" 표시
  
  lcd.print(t,1); // 온도값 LCD로 출력
  lcd.print(" C"); // 온도 단위 표시
  lcd.setCursor(0,1); //LCD 커서 줄바꿈
  lcd.print("HUMI: "); //LCD 2번째 줄에 "humidity:" 출력

  lcd.print(h); //습도값 LCD에 출력
  lcd.print(" %"); //습도 단위 출력
}
```

<br>
<br>

## 아두이노 DHT,FAN,HC-06 코드 및 회로도
<br>
<br>
<div align=center>

![5](https://user-images.githubusercontent.com/73435598/206869355-e076e539-1158-42cd-9df8-175303a88673.png)

</div>

<br>
<br>

```c
#include <SoftwareSerial.h>
#include<Servo.h>
#include<LiquidCrystal_I2C.h>
#include <Wire.h>
#include "DHT.h"
 
#define LED1  9 //LED핀 번호
#define LED2 10 
#define BTtx 8   // 블루투스 tx핀이 연결된 아두이노 핀 번호
#define BTrx 7 // 블루투스 rx핀이 연결된 아두이노 핀 번호




#define DHTPIN 2
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);
int FAN = 5;
int FAN2 = 6;


SoftwareSerial BT(BTtx, BTrx);  //소프트웨어 시리얼 객체

Servo myservo1;
Servo myservo2;

char data = 0;  //모바일 앱으로부터 입력받은 값 저장할 변수

void setup(){
  dht.begin();
  BT.begin(9600);  //소프트웨어 시리얼 통신 준비
  Serial.begin(9600);  //하드웨어 시리얼 통신 준비
  myservo1.attach(3);
  myservo2.attach(4);
  

  pinMode(LED2, OUTPUT);
  pinMode(LED1, OUTPUT);  //4번핀 출력 모드
  
  
  pinMode(FAN,OUTPUT);
  pinMode(FAN2,OUTPUT);

 
  
}


void loop(){
  float h = dht.readHumidity();
  float t = dht.readTemperature();

  if(isnan(h) || isnan(t)){
    Serial.println(F("Failed to read from DHT sensor!"));
    return;
  }
  Serial.print((int)t); Serial.print("*C ");
  Serial.print((int)h); Serial.println("%");

  delay(5000);

  if(BT.available() > 0){  //블루투스 통신으로 입력된 데이터가 있으면
      data = BT.read();    //입력된 데이터를 읽어서 변수에 저장하기
  }

  
  if(data == 'a'){  
      digitalWrite(LED2, HIGH);  //LED 켜기
      digitalWrite(LED1, HIGH);  //LED 켜기
  }
  
  else if(data == 'b'){  
      digitalWrite(LED2, LOW);  //LED 끄기
      digitalWrite(LED1, LOW);  //LED 끄기
      
  }

  else if (data == 'c'){
    myservo2.writeMicroseconds(1700);
    myservo1.writeMicroseconds(1300);
    delay(1000);  
    myservo2.writeMicroseconds(1500);
    myservo1.writeMicroseconds(1500); 
  }

  
  data = 0;  //data 초기화

  if(((float)h)>=60){
    digitalWrite(FAN,HIGH);
    digitalWrite(FAN2,HIGH);
  }
  else if(((float)h)<=35){
     digitalWrite(FAN,LOW);
     digitalWrite(FAN2,LOW);
  }

}
```

<br>
<br>
 
 ## referance
 
 
[Wiki](https://github.com/jojun01835/2022-portportfolio/wiki/referance)



