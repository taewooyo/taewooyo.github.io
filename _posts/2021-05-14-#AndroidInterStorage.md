---
layout: post
title: "�뙆�씪 泥섎━(sd移대뱶)"
categories: �븞�뱶濡쒖씠�뱶
author: bn-tw2020
---
* content
{:toc}


## Intro

```
�븞�뱶濡쒖씠�뱶 �뙆�씪泥섎━�쓽 湲곕뒫 以� �궡遺�����옣�냼 sd移대뱶�뿉 ����옣�빐蹂닿린
```





---

## SD 移대뱶

```
SD移대뱶�뒗 �슂利섏뿉 �궗�슜�븯吏� �븡�뒗 異붿꽭�씠�떎. �뵆�옒�떆 硫붾え由щ�� 二쇰줈 �궗�슜�븳�떎.
理쒓렐�뿉 �굹�삤�뒗 �뒪留덊듃�룿��� SD移대뱶瑜� 吏��썝�븯吏� �븡�뒗�떎.
洹� �씠�쑀, SD移대뱶�뒗 �궡�옣 硫붾え由ъ뿉 鍮꾪빐�꽌 �뒓由� �렪�씠�떎. 
洹몃옒�꽌 �뒓�젮吏��뒗 寃껋뿉 而⑦뵆�젅�씤�씠 留롮븘�꽌 �뜲�씠�꽣瑜� �젣怨듯븯�뒗�뜲 �뒓由� �렪�씠湲곗뿉 �젣怨듭쓣 �븯吏� �븡�쑝�젮怨� �빀�땲�떎.
```


## SD 移대뱶�뿉�꽌 �뙆�씪 �씫湲�

```
1. Device File Explorer�뿉�꽌 /sdcard �샊��� /storage/emulated/0 �뤃�뜑�뿉 �뀓�뒪�듃 �뙆�씪 �깮�꽦�븯湲�
2. AndroidManifest.xml�뙆�씪�뿉 SD 移대뱶瑜� �궗�슜�븷 �닔 �엳�룄濡� �띁誘몄뀡 諛� application�뿉 愿��젴�맂 �냽�꽦 異붽��
    <use-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE/>
    <application android:requestLegacyExternalStorage="true">

3. target SDK 踰꾩쟾 蹂�寃�
    target 30踰꾩쟾�뿉�꽌�뒗 SD 泥섎━ 諛⑸쾿�씠 ����룺 蹂�寃쎈릺�뼱 �젒洹� 嫄곕��媛� 諛쒖깮�븷 �닔 �엳�떎.
    �씠 寃쎌슦�뿉�뒗 踰꾩쟾�쓣 target 29踰꾩쟾�쑝濡� 泥섎━�빐二쇰㈃ �맂�떎.
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
            android:text="SD 移대뱶�뿉�꽌 �뙆�씪 �씫�뼱�삤湲�" />

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


## SD 移대뱶�뿉�꽌 �뤃�뜑 �벐湲�

```
1. Environment �겢�옒�뒪�쓽 �젙�쟻 硫붿냼�뱶瑜� �씠�슜�빐 SD移대뱶�쓽 �룞�옉 �뿬遺� 諛� 愿��젴 �뤃�뜑 寃쎈줈 援ы븳�떎.
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
            android:text="SD 移대뱶�뿉 �뵒�젆�넗由� �깮�꽦" />

        <Button
            android:id="@+id/btnRmdir"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="SD 移대뱶�뿉�꽌 �뵒�젆�넗由� �궘�젣" />

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

## �듅�젙 �뤃�뜑�쓽 �븯�쐞 �뤃�뜑 諛� �뙆�씪 �젒洹�

```
1. 吏��젙�븳 �뤃�뜑�쓽 �븯�쐞 �뤃�뜑 諛� �뙆�씪 紐⑸줉�뿉 �젒洹쇳븯湲�
    �듅�젙 �뤃�뜑�쓽 �븯�쐞 �뤃�뜑 諛� �뙆�씪 紐⑸줉��� File.listFiles()硫붿냼�뱶 �궗�슜�븯�뿬 �젒洹쇳븳�떎.
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
            android:text="�떆�뒪�뀥 �뤃�뜑�쓽 �뤃�뜑/�뙆�씪 紐⑸줉" />

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
                        strFname = "<�뤃�뜑> " + sysFiles[i].toString();
                    else
                        strFname = "<�뙆�씪> " + sysFiles[i].toString();

                    binding.edtFilelist.setText(binding.edtFilelist.getText() + "\n" + strFname);
                }
            }
        });
    }
}
```