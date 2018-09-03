---
title: "Android Custom Font"
date: 2018-09-03 13:11:00 -0400
categories: android font 
---



https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml


## 1. 개요
Android 8.0 (API레벨 26)에서는 글꼴을 리소스로 사용할 수 있는 XML의 새로운 기능인 fonts in XML이 도입되었다.  
res/font/ 폴더에 글꼴파일을 추가하면 안드로이드 스튜디오에서 사용할 수 있게 R.java 파일에 컴파일된다.  
그래서 글꼴 자원을 사용하려면 @font/글꼴파일명 또는 R.font.글꼴파일명 처럼 쓰면 된다.  
추가로 Android 4.1 (API레벨 16)이상에서 실행하는 단말에서 XML의 글꼴 기능을 사용하려면 Support Library 26을 사용하면 된다.  



## 2. 글꼴추가하기 ##
1. res 폴더에서 마우스 오른쪽버튼을 클릭하고 New > Android resource directory 를 선택한다.
2. Resource Type에서 'font'를 선택한다.
3. 폰트파일을 font 폴더에 추가한다.
4. 폰트파일을 더블클릭하면 미리보기 할 수 있다.



## 3. Creating a font family
font family는 스타일 및 굵기를(weight)를 함께 구성하는 세트다.  
이렇게 하면 하나의 리소스를 폰트를 제어할 수 있다.  

1. 폰트패밀리 추가
font 폴더에서 마우른 오른버튼 클릭하고 New > Font resource file '폰트패밀리명.xml' 입력 후 'OK'. 
res/font/폰트패밀리명.xml 파일이 생성되면 아래의 내용을 입력한다.
```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:app="http://schemas.android.com/apk/res-auto">
    <font 
    	app:fontStyle="normal" 
    	app:fontWeight="400" 
    	app:font="@font/myfont-Regular"/>
    <font 
    	app:fontStyle="italic" 
    	app:fontWeight="400" 
    	app:font="@font/myfont-Italic" />
</font-family>
```

2. XML로 폰트패밀리 적용
```xml
<TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="@font/폰트패밀리명"/>
```

3. 프로그램으로 폰트적용
```java
Typeface typeface = ResourcesCompat.getFont(context, R.font.myfont);
```


## 4. 테마를 이용한 프로젝트 전체에 적용
1. Custom 폰트스타일 정의 
여기에서는 TextView, Button, EditText, RadioButton, CheckBox에 나타나는 폰트만 적용하였다.
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="OneShinhanTextViewStyle" parent="@android:style/Widget.DeviceDefault.TextView">
        <item name="android:fontFamily">@font/one_shinhan</item>
    </style>
    <style name="OneShinhanButtonStyle" parent="@android:style/Widget.DeviceDefault.Button">
        <item name="android:fontFamily">@font/one_shinhan</item>
    </style>
    <style name="OneShinhanEditTextStyle" parent="@android:style/Widget.DeviceDefault.EditText">
        <item name="android:fontFamily">@font/one_shinhan</item>
    </style>
    <style name="OneShinhanRadioButtonStyle" parent="@android:style/Widget.DeviceDefault.CompoundButton.RadioButton">
        <item name="android:fontFamily">@font/one_shinhan</item>
    </style>
    <style name="OneShinhanCheckboxStyle" parent="@android:style/Widget.DeviceDefault.CompoundButton.CheckBox">
        <item name="android:fontFamily">@font/one_shinhan</item>
    </style>
</resources>
```
2. AppTheme에 Customfont 적용
```xml
<style name="AppTheme" parent="Theme.AppCompat.NoActionBar" >
	...	
    <!-- custom font-->
    <item name="android:buttonStyle">@style/OneShinhanButtonStyle</item>
    <item name="android:editTextStyle">@style/OneShinhanEditTextStyle</item>
    <item name="android:radioButtonStyle">@style/OneShinhanRadioButtonStyle</item>
    <item name="android:checkboxStyle">@style/OneShinhanCheckboxStyle</item>
    <item name="android:textViewStyle">@style/OneShinhanTextViewStyle</item>
    ...
</style>
```








[ 참고 자료 ] . 

기존에 Application 레벨에서 적용하는 방법 . 

https://medium.com/@rujulsolanki1993/implementing-custom-fonts-in-android-on-application-level-f0784d4348f8
