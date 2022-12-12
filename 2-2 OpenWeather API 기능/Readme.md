# 2022-portportfolio
<br>

**OpenWeather API 사용**

<br>
<br>

**API 사용법**

<br>

* 현재 날씨를 받을 수 있는 Current weather data를 선택한다.
* Current weather data 호출 방식 중 도시 이름으로 API call 은 아래와 같다.

<br>
<br>

```
api.openweathermap.org/data/2.5/weather?q={도시이름}&appid={API key}
```

<div align=center>

![1](https://user-images.githubusercontent.com/73435598/206702608-a79e0cd0-1e9d-4cc0-bcb1-c81ae77b74fa.png)

</div>

<br>
<br>

**2. API JSon**

<br>

* OpenWeater API는 Json형식으로 불러와진다

<br>

<div align=center>

![2](https://user-images.githubusercontent.com/73435598/206702756-5f68614b-d0e5-478e-9387-5422cadaae72.png)

</div>

<br>
<br>

각 데이터들의 세부사항은 [Json referance](https://openweathermap.org/weather-data) 참조

**3. 이미지 불러오기**

* OpenWeater API는 해당 하는 날씨의 이미지기능도 지원한다

<br>
<br>

**example**

<br>

```
this.img_url='http://openweathermap.org/img/wn/'+this.W_state.icon+'@2x.png';
this.W_state.icon : get으로 가져온 데이터에서 아이콘 정보(weather.icon from response)를 가져와야한다
```

<div align=center>

![3](https://user-images.githubusercontent.com/73435598/206703196-96a1bc01-9872-4283-9918-9c2d91ac85df.png)

</div>

<br>
<br>

## referance

[Wiki](https://github.com/jojun01835/2022-portportfolio/wiki/referance)
