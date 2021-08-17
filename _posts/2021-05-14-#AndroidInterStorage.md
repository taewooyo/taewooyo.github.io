---
layout: post
title: "파일 처리(sd카드)"
categories: 안드로이드
author: bn-tw2020
---
 * content
 {:toc}

 
  ## Intro

  ```
 안드로이드 파일처리의 기능 중 내부저장소 sd카드에 저장해보기
 ```

 
 
 
 
  ---

  ## SD 카드

  ```
 SD카드는 요즘에 사용하지 않는 추세이다. 플래시 메모리를 주로 사용한다.
 최근에 나오는 스마트폰은 SD카드를 지원하지 않는다.
 그 이유, SD카드는 내장 메모리에 비해서 느린 편이다. 
 그래서 느려지는 것에 컨플레인이 많아서 데이터를 제공하는데 느린 편이기에 제공을 하지 않으려고 합니다.
 ```

 
  ## SD 카드에서 파일 읽기

  ```
 1. Device File Explorer에서 /sdcard 혹은 /storage/emulated/0 폴더에 텍스트 파일 생성하기
 2. AndroidManifest.xml파일에 SD 카드를 사용할 수 있도록 퍼미션 및 application에 관련된 속성 추가
     <use-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE/>
     <application android:requestLegacyExternalStorage="true">

 3. target SDK 버전 변경
     target 30버전에서는 SD 처리 방법이 대폭 변경되어 접근 거부가 발생할 수 있다.
     이 경우에는 버전을 target 29버전으로 처리해주면 된다.
 ```

  ```xml
 <?xml version="1.0" encoding="utf-8"?>
 <layout>

      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         xmlns:app="http://schemas.android.com/apk/res-auto"
         xmlns:tools="http://schemas.android.com/tools"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         tools:context=".MainActivity">

          <Button
             android:id="@+id/btnRead"
             android:layout_width="match_parent"
             android:layout_height="wrap_content"
             android:text="SD 카드에서 파일 읽어오기" />

          <EditText
             android:id="@+id/edtSD"
             android:layout_width="match_parent"
             android:layout_height="wrap_content"
             android:lines="18" />

      </LinearLayout>
 </layout>
 ```

  ```java
 public class MainActivity extends AppCompatActivity {
     ActivityMainBinding binding;

     @Override
     protected void onCreate(Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);
         binding = DataBindingUtil.setContentView(this, R.layout.activity_main);


         ActivityCompat.requestPermissions(this, new String[] {Manifest.permission.WRITE_EXTERNAL_STORAGE}, MODE_PRIVATE);

         binding.btnRead.setOnClickListener(new View.OnClickListener() {
             @Override
             public void onClick(View v) {
                 try {
                     FileInputStream fileInputStream = new FileInputStream("/storage/emulated/0/sd_text.txt");
                     byte[] txt = new byte[fileInputStream.available()];
                     fileInputStream.read(txt);
                     binding.edtSD.setText(new String(txt));
                     fileInputStream.close();
                 } catch (IOException ignored) {

                 }
             }
         });
     }
 }
 ```

 
  ## SD 카드에서 폴더 쓰기

  ```
 1. Environment 클래스의 정적 메소드를 이용해 SD카드의 동작 여부 및 관련 폴더 경로 구한다.
 ```

  ```xml
 <?xml version="1.0" encoding="utf-8"?>
 <layout>

      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         xmlns:app="http://schemas.android.com/apk/res-auto"
         xmlns:tools="http://schemas.android.com/tools"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         android:orientation="vertical"
         tools:context=".MainActivity">

          <Button
             android:id="@+id/btnMkdir"
             android:layout_width="match_parent"
             android:layout_height="wrap_content"
             android:text="SD 카드에 디렉토리 생성" />

          <Button
             android:id="@+id/btnRmdir"
             android:layout_width="match_parent"
             android:layout_height="wrap_content"
             android:text="SD 카드에서 디렉토리 삭제" />

      </LinearLayout>
 </layout>
 ```

  ```java
 public class MainActivity extends AppCompatActivity {
     ActivityMainBinding binding;

     @Override
     protected void onCreate(Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);
         binding = DataBindingUtil.setContentView(this, R.layout.activity_main);


         ActivityCompat.requestPermissions(this, new String[] {Manifest.permission.WRITE_EXTERNAL_STORAGE}, MODE_PRIVATE);

         final String strSDpath = Environment.getExternalStorageDirectory().getAbsolutePath();
         final File myDir = new File(strSDpath + "/mydir");

         binding.btnMkdir.setOnClickListener(new View.OnClickListener() {
             @Override
             public void onClick(View v) {
                 myDir.mkdir();
             }
         });
         binding.btnRmdir.setOnClickListener(new View.OnClickListener() {
             @Override
             public void onClick(View v) {
                 myDir.delete();
             }
         });
     }
 }
 ```

  ## 특정 폴더의 하위 폴더 및 파일 접근

  ```
 1. 지정한 폴더의 하위 폴더 및 파일 목록에 접근하기
     특정 폴더의 하위 폴더 및 파일 목록은 File.listFiles()메소드 사용하여 접근한다.
 ```

  ```xml
 <?xml version="1.0" encoding="utf-8"?>
 <layout>

      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         xmlns:app="http://schemas.android.com/apk/res-auto"
         xmlns:tools="http://schemas.android.com/tools"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         android:orientation="vertical"
         tools:context=".MainActivity">

          <Button
             android:id="@+id/btnFilelist"
             android:layout_width="match_parent"
             android:layout_height="wrap_content"
             android:text="시스템 폴더의 폴더/파일 목록" />

          <EditText
             android:id="@+id/edtFilelist"
             android:layout_width="match_parent"
             android:layout_height="wrap_content"/>

      </LinearLayout>
 </layout>
 ```

  ```java
 public class MainActivity extends AppCompatActivity {
     ActivityMainBinding binding;

     @Override
     protected void onCreate(Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);
         binding = DataBindingUtil.setContentView(this, R.layout.activity_main);


         ActivityCompat.requestPermissions(this, new String[] {Manifest.permission.WRITE_EXTERNAL_STORAGE}, MODE_PRIVATE);

         binding.btnFilelist.setOnClickListener(new View.OnClickListener() {
             @Override
             public void onClick(View v) {
                 String sysDir = Environment.getRootDirectory().getAbsolutePath();
                 File[] sysFiles = (new File(sysDir).listFiles());

                 String strFname;
                 for (int i = 0; i < sysFiles.length; i++) {
                     if (sysFiles[i].isDirectory() == true)
                         strFname = "<폴더> " + sysFiles[i].toString();
                     else
                         strFname = "<파일> " + sysFiles[i].toString();

                     binding.edtFilelist.setText(binding.edtFilelist.getText() + "\n" + strFname);
                 }
             }
         });
     }
 }
 ```
