---
layout: post
title: "카메라 기능과 파이어베이스 이미지 저장"
categories: 안드로이드
author: bn-tw2020
---
* content
{:toc}

## Camera & FirebaseStorage

```
안드로이드에서 사진을 찍고 갤러리에 저장하고, 파이어베이스 저장소에 이미지 저장하는 방법
```




----

## activity_main

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/takePicture"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:text="사진 촬영"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@id/save"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/save"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="10dp"
        android:text="파이어베이스 저장"
        app:layout_constraintTop_toTopOf="@id/takePicture"
        app:layout_constraintStart_toEndOf="@id/takePicture"
        app:layout_constraintEnd_toStartOf="@id/gallery"
        />

    <Button
        android:id="@+id/gallery"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="10dp"
        android:text="gallery"
        app:layout_constraintTop_toTopOf="@id/save"
        app:layout_constraintStart_toEndOf="@id/save"
        app:layout_constraintEnd_toEndOf="parent"
        />

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="411dp"
        android:layout_height="663dp"
        app:layout_constraintTop_toBottomOf="@id/takePicture"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>


</androidx.constraintlayout.widget.ConstraintLayout>
```

```
사진찍기 버튼, 갤러리에서 이미지 가져오기, 파이어베이스 저장하는 버튼을 구현하는 뷰
```

## MainActivity

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    final static int TAKE_PICTURE = 1;
    final static int GET_GALLERY_IMAGE = 2;

    Button takePicture;
    Button save;
    Button gallery;
    ImageView imageview;
    Uri selectedImageUri;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        takePicture = findViewById(R.id.takePicture);
        save = findViewById(R.id.save);
        gallery = findViewById(R.id.gallery);
        imageview = findViewById(R.id.imageView);

        takePicture.setOnClickListener(this);
        gallery.setOnClickListener(this);
        save.setOnClickListener(this);

        // 마시멜로우 이상일 경우에는 권한 체크 후 권한 요청
       if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            if(checkSelfPermission(Manifest.permission.CAMERA) == PackageManager.PERMISSION_GRANTED && checkSelfPermission(Manifest.permission.READ_EXTERNAL_STORAGE)
                    ==checkSelfPermission(Manifest.permission.WRITE_EXTERNAL_STORAGE)){}
            else{
                ActivityCompat.requestPermissions(this,new String[]{Manifest.permission.CAMERA,Manifest.permission.WRITE_EXTERNAL_STORAGE,Manifest.permission.READ_EXTERNAL_STORAGE},1 );
            }
        }
    }

    // 권한 요청
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if(grantResults[0] == PackageManager.PERMISSION_GRANTED && grantResults[1] == PackageManager.PERMISSION_GRANTED && grantResults[2] == PackageManager.PERMISSION_GRANTED){
            Log.d("로그", "Permission: " + permissions[0] + " was " + grantResults[0]);
        }
    }

    // 버튼 이벤트 리스너
    @Override
    public void onClick(View v) {
        Intent intent;
        switch(v.getId()) {
            case R.id.takePicture: // 사진 찍기
                intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
                startActivityForResult(intent, TAKE_PICTURE);
                break;
            case R.id.gallery: // 갤러리 들어가기
                intent = new Intent(Intent.ACTION_PICK);
                intent.setDataAndType(android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI, "image/*");
                startActivityForResult(intent, GET_GALLERY_IMAGE);
                break;
            case R.id.save: // 파이어베이스 이미지 업로드
                clickUpload();
                selectedImageUri = null;
                break;
        }
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        // 사진 촬영 완료 후 응답
        if(requestCode == TAKE_PICTURE) {
            if(resultCode == RESULT_OK && data.hasExtra("data")) {
                Bitmap bitmap = (Bitmap) data.getExtras().get("data");
                if(bitmap != null) imageview.setImageBitmap(bitmap);

                String imageSaveUri = MediaStore.Images.Media.insertImage(getContentResolver(), bitmap, "사진 저장", "찍은 사진이 저장되었습니다.");
                selectedImageUri = Uri.parse(imageSaveUri);
                Log.d(TAG, "MainActivity - onActivityResult() called" + selectedImageUri);
            }
        }

        // 갤러리에서 이미지 가져온 후의 응답
        else if(requestCode == GET_GALLERY_IMAGE){
            if(resultCode == RESULT_OK && data.getData() != null) {
                selectedImageUri = data.getData();
                Log.d(TAG, "MainActivity - onActivityResult() called" + selectedImageUri);
                Log.d(TAG, "MainActivity - onActivityResult() called" + getRealPathFromURI(selectedImageUri));

                Glide.with(this)
                        .load(getRealPathFromURI(selectedImageUri))
                        .into(imageview);

            }
        }
    }

    // 절대 경로로 변경
    public String getRealPathFromURI(Uri contentUri) {

        String[] proj = { MediaStore.Images.Media.DATA };

        Cursor cursor = getContentResolver().query(contentUri, proj, null, null, null);
        cursor.moveToNext();
        String path = cursor.getString(cursor.getColumnIndex(MediaStore.MediaColumns.DATA));
        Uri uri = Uri.fromFile(new File(path));

        cursor.close();
        return path;
    }

    // 파이어베이스 업로드 함수
    public void clickUpload() {
        
        // 1. FirebaseStorage을 관리하는 객체 얻어오기
        FirebaseStorage firebaseStorage= FirebaseStorage.getInstance();

        // 2. 업로드할 파일의 node를 참조하는 객체
        // 파일 명이 중복되지 않도록 날짜를 이용
        SimpleDateFormat sdf= new SimpleDateFormat("yyyyMMddhhmmss");
        String filename= sdf.format(new Date())+ ".png";
        // 현재 시간으로 파일명 지정 20191023142634
        // 원래 확장자는 파일의 실제 확장자를 얻어와서 사용해야함. 그러려면 이미지의 절대 주소를 구해야함.

        StorageReference imgRef= firebaseStorage.getReference("uploads/"+filename);
        // uploads라는 폴더가 없으면 자동 생성

        // 참조 객체를 통해 이미지 파일 업로드
        // imgRef.putFile(imgUri);
        // 업로드 결과를 받고 싶다면 아래와 같이 UploadTask를 사용하면 된다.
        UploadTask uploadTask =imgRef.putFile(selectedImageUri);
        uploadTask.addOnSuccessListener(new OnSuccessListener<UploadTask.TaskSnapshot>() {
            @Override
            public void onSuccess(UploadTask.TaskSnapshot taskSnapshot) {
                Toast.makeText(MainActivity.this, "success upload", Toast.LENGTH_SHORT).show();
            }
        })
                .addOnFailureListener(new OnFailureListener() {
                    @Override
                    public void onFailure(@NonNull Exception e) {
                        Log.d(TAG, "MainActivity - onFailure() called");
                    }
                });
    }
}

```

```
1. 사진을 찍거나, 갤러리에서 이미지 가져오는 부분은 intent로 들어갓다가 onActivityResult로 응답을 받는다.
2. 파이어 베이스에 이미지를 업로드하는 방법은
   FirebaseStorage을 관리하는 객체를 가져온 후,
   업로드할 파일의 참조하는 객체를 통해 이미지 파일 업로드를 한다.
   파일 명은 중복되지 않도록 현재 시간을 구하여 업로드를 진행하였다.

3. 현재 확장자 명을 png로 고정하였지만, 실제는 위에서 만든 절대 경로로 변환하는 것에서 확장자를 가져와서 구현해야한다.
```

## Summary

```
1. 이미지 하나를 올리는데 많은 과정으로 변환해하는 것에 어렵구나 라고 느꼈다.
2. 더 좋은 방법이 있으면 새롭게 구현해서 구조를 정리해보고 싶다.
```