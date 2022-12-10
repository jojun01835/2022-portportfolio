# 2022-portportfolio
<br>

**Android 사용한 기술**

<br>
<br>

**Blue Tooth**

<br>

* 안드로이드는 블루투스를 지원하여 스마트폰이 다른 블루투스 기기와 무선으로 통신할 수 있습니다.<br>
  통신은 Android Bluetooth API를 통해서 이루어지는데, 이를 사용해서 다음과 같은 작업을 수행할 수 있습니다.<br>

1. 다른 블루투스 기기 스캔(기기 검색, 페어링된 기기확인)<br>

2. 블루투스 기기 연결<br>

3. 연결된 기기간 데이터 전송 및 수신<br>

4. 블루투스 프로필<br>
<br>
<br>
<br>

**1. 기본적인 권한**<br>

* 블루투스 기능들을 사용하기 위해서 기본적으로 선언해야 하는 권한이 있습니다.<br><br>

```
<!-- Android 11 버전까지를 위한 권한 -->
<uses-permission
    android:name="android.permission.BLUETOOTH"
    android:maxSdkVersion="30" />
<uses-permission
    android:name="android.permission.BLUETOOTH_ADMIN"
    android:maxSdkVersion="30" />

<!--기기검색 권한-->
<uses-permission android:name="android.permission.BLUETOOTH_SCAN"
    android:usesPermissionFlags="neverForLocation"
    tools:targetApi="s" />
<!--페어링된 기기 확인 권한->
<uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
```
<br>
기기 검색을 위한 권한, 페어링된 기기를 확인하기 위한 권한을 지정해야만 사용할 수 있습니다.
<br>
<br>

**2.블루투스 설정(블루투스 활성화, 블루투스 비활성화)**<br><br>

* 블루투스를 사용하기 위해서는 기기에서 블루투스가 지원되는지 확인하고 지원된다면 활성화되어 있는지 확인해야 합니다.<br>
  블루투스가 활성화 되어 있는지 확인하기 위해서는 BluetoothManager, BluetoothAdapter 두 클래스를 이용해야 합니다. 두 클래스는 다음과 같은 역할을 수행합니다.<br>
 
 * BluetoothManager: BluetoothAdapter 객체를 획득할 때 사용하는 클래스로 전체적인 블루투스 관리를 수행합니다.
 * BluetoothAdapter: 모든 Bluetooth 상호작용의 진입점입니다. 이를 사용해 다른 Bluetooth 장치를 검색하고, 페어링된 장치 목록을 쿼리하고, MAC 주소를 사용하여<br>              Bluetooth 기기를 인스턴스화 할 수 있고, 다른 기기와 통신을 위해 Socket을 생성할 수 있습니다.<br><br>
 
 **기기 검색과 페어링**<br><br>
 
  * 기기 검색과 페어링 모두 BluetoothAdapter 클래스를 사용해서 수행합니다. <br>
  * 기기 검색은 로컬 영역에서 Bluetooth 지원 장치를 검색하는 것으로 검색이 가능하면 해당 기기의 이름,클래스 및 고유 MAC 주소와 같은 정보를 공유하여 검색 요청에 응답합니다.<br> 페어링은 처음으로 기기와 연결될 때 이루어지는 것으로, 페어링이 되면 해당 기기에 대한 정보가 저장됩니다.<br>

<br>
<br>

**페어링된 기기 검색**<br><br>

* 기기와 처음으로 연결되면 페어링 요청이 자동으로 사용자에게 제공됩니다.<br>
* 기기가 페어링되면 기기 이름, 클래스 및 MAC 주소와 같은 해당 기기에 대한 정보가 자동으로 저장됩니다. 저장된 MAC 주소를 사용하면 언제든지 연결을 시작할 수 있습니다.<br>
* 페어링되었다는 것은 두 기기가 서로의 존재를 인식하고 인증에 사용할 수 있는 공유 링크 키를 가지고 있으며 서로 암호화된 연결을 설정할 수 있음을 의미합니다.<br>
* 페어링된 기기는 BluetoothAdapter.getBoundedDevices() 메서드를 통해서 획득할 수 있습니다.<br>

**주변 기기 검색**<br><br>

* 기기 검색의 경우 BluetoothAdapter.startDiscovery() 메서드를 통해서 수행합니다.<br>
* startDiscovery() 메서드는 비동기 프로세스이며 검색이 성공적으로 시작했는지 여부를 나타내는 Boolean 값을 즉시 반환합니다.<br>
* 검색 프로세스는 약 12초 정도 이루어집니다.<br>
* 검색에 대한 결과인 기기에 대한 정보를 받으려면 BroadcastReceiver를 사용해야 합니다.<br><br>

**데이터 전송**<br><br>

* 블루투스 연결에 성공하면 BluetoothSocket을 사용해서 데이터를 주고받을 수 있습니다.<br>
* BloetoothSocket의 InputStream은 데이터를 받을 때 사용하고, OutputStream은 데이터를 전송할 때 사용합니다.<br>
* 데이터를 주고받을 때 주의할점은 이 또한 blocking 메서드를 사용하므로 메인 스레드에서 수행하는것이 아니라 작업 스레드를 만들어서 수행해야 합니다.<br><br>

**데이터를 주고 받는 코드(예제)**
```
private inner class ConnectedThread(private val bluetoothSocket: BluetoothSocket) : Thread() {
    private lateinit var inputStream: InputStream
    private lateinit var outputStream: OutputStream

    init {
        try {
            // BluetoothSocket의 InputStream, OutputStream 초기화
            inputStream = bluetoothSocket.inputStream
            outputStream = bluetoothSocket.outputStream
        } catch (e: IOException) {
            setLog(TAG, e.message.toString())
        }
    }

    override fun run() {
        val buffer = ByteArray(1024)
        var bytes: Int

        while (true) {
            try {
                // 데이터 받기(읽기)
                bytes = inputStream.read(buffer)
                setLog(TAG, bytes.toString())
            } catch (e: Exception) { // 기기와의 연결이 끊기면 호출
                setLog(TAG, "기기와의 연결이 끊겼습니다.")
                break
            }
        }
    }

    fun write(bytes: ByteArray) {
        try {
            // 데이터 전송
            outputStream.write(bytes)
        } catch (e: IOException) {
            setLog(TAG, e.message.toString())
        }
    }

    fun cancel() {
        try {
            bluetoothSocket.close()
        } catch (e: IOException) {
            setLog(TAG, e.message.toString())
        }
    }
}
```

<br>
<br>
<br>

### 안드로이드 Viewpager2
<br>
<br>
<br>

* 좌우로 페이지를 넘기는 형식으로 사용되며 앱 소개 페이지, 배너 등에서 자주 사용됩니다.<br>
  ViewPager2에서는 기존 ViewPager 와는 다르게 RecyclerView를 기반으로 빌드되었습니다.<br><br><br>

<div align=center> 

![1](https://user-images.githubusercontent.com/73435598/206853763-67589f39-79bb-486d-b1a8-eea77a8e2b97.png)

</div>

<br>
<br>

* ViewPager2는 ViewGroup을 상속받았고 initialize 메서드에서 RecyclerView를 생성하는 것을 확인 할 수 있습니다.<br><br>

```
ublic final class ViewPager2 extends ViewGroup {
    private void initialize(Context context, AttributeSet attrs) {
        mRecyclerView = new RecyclerViewImpl(context);
        mLayoutManager = new LinearLayoutManagerImpl(context);
        mRecyclerView.setLayoutManager(mLayoutManager);
        mPagerSnapHelper = new PagerSnapHelperImpl();
        mPagerSnapHelper.attachToRecyclerView(mRecyclerView);
        . . .
	}
}
```

<br>
<br>

* 그리고 LinearLayoutManager와 PageSnapHelper를 설정하는것을 확인 할 수 있습니다.<br>
* ViewPager2에서는 RecylerView.Adapter의 기능들을 이용 할 수 있습니다.<br>
* 한 가지 아쉬운 점은 ViewPager2는 final class로 선언되어있기 때문에 Custom ViewPager2를 만들 수 없습니다.<br>

<br>
<br>

**ViewPager2에 새롭게 추가된 기능은 다음과 같습니다.**<br><br>

* RTL (right to left) layout support<br>
* Vertical orientation support<br>
* Reliable Fragment support<br>
* Dataset change animations<br>

<br>
<br>

## WebView<br><br>

* 웹뷰(WebView)란 프레임워크에 내장된 웹 브라우저 컴포넌트로 뷰(View)의 형태로 앱에 임베딩하는 것을 말합니다.<br>
* 즉, WebView는 앱 내에 웹 브라우저를 넣는 것입니다.<br>
* 웹 페이지를 보기 위해서 혹은 앱 안에서 HTML을 호출하여 앱을 구현하는 하이브리드 형태의 앱을 개발하는데에도 많이 사용됩니다.<br>

<br>
<br>

**장점**<br><br>
* 하이브리드 앱은 안드로이드 네이티브 앱 개발에 비해서 개발이 비교적 쉽습니다.<br>

* 특히 기기간의 호환성을 해결하기가 상대적으로 편합니다.<br>

* 타 웹 사이트 링크로 가는 기능등을 지원하기 위해서 많이 사용됩니다.<br><br>

**단점**<br><br>

* HTML 기반인 만큼 상대적으로 반응성이 약하고, 애니메이션등의 다양한 UI 효과를 넣기 어렵습니다.<br>

* OS에 맞게 일부 기능들을 제외하고 작게 만든 웹 브라우저로 HTML5 호환성 등 기능의 제약을 많이 가지고 있습니다.<br><br>

**개발 참고 사항** <br><br>

1. 인터넷 접근 권한 허용 <br><br>

AndroidManifest.xml 파일에서 manifest 태그 안에 <uses-permission android:name="android.permission.INTERNET" />를 추가해야 앱에서 인터넷을 통신을 사용할 수 있다.<br><br><br>

2. 에러 <br><br>

* 에러: ERR_CLEARTEXT_NOT_PERMITTED <br><br>

* 원인<br>

웹뷰가 실행되었지만, ERR_CLEARTEXT_NOT_PERMITTED와 같은 에러가 발생할 때가 있다. <br>

그 이유는 URL을 통해서 웹 페이지를 로드하는데 https가 아닌 http 프로토콜을 사용하여 보안이 취약하기 때문이다.<br>

* 해결방법<br>

이를 해결하기 위해서는 AndroidManifest.xml 파일에서 application태그 속성으로 android:usesCleartextTraffic="true"를 추가해야 한다.<br><br>

```
<application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.SearchDeliveryInfoApplication"
        android:usesCleartextTraffic="true">
```

<br>
<br>

2-1 에러: 앱이 아닌 기본 웹 어플리케이션에서 열리는 웹 페이지<br><br>

* 해결방법<br>

내가 만든 WebView의 속성 값 중 webViewClient를 안드로이드 SDK에서 제공하는 WebViewClient로 덮어씌운다.<br>

```
binding.webView.webViewClient = WebViewClient()
...
binding.webView.loadUrl("https://google.com")
```
<br>
<br>

2-2 에러: 웹뷰에서 보여지는 웹 페이지의 버튼 작동 X <br><br>

* 원인<br>

Android에서는 기본적으로 보안을 위해 JavaScript를 허용하지 않는다.<br>

* 해결방법<br>

WebView를 초기화 할 때, JavaScript 사용을 허용한다.<br><br>

```
binding.webView.settings.javaScriptEnabled = true
```

<br>
<br>
### referance
<br>
<br>
<br>
