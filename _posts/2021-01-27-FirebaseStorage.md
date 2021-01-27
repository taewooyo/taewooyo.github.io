---
layout: post
title: "Firebase Stoarge"
categories: 안드로이드
author: bn-tw2020
---
* content
{:toc}

## FirebaseStorage

```
FirebaseSotrage는 일종의 문서, 사진, 파일, 동영상을 저장하는 저장소라고 생각하면 될 것 같다.
```




----

## Firebase

* Firebase 홈페이지들어가서 기본적으로 프로젝트를 생성하고 Storage까지 생성을 하게 되면 아래와 같은 곳을 확인 할 수 있다.

* 생성된 stoarge에 저장된 이미지를 가져오는 것이다.

<img width="947" alt="스크린샷 2021-01-27 오후 10 46 09" src="https://user-images.githubusercontent.com/66770613/105999801-73bbbb80-60f1-11eb-8311-6ecba884945c.png">


```java 
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                // 2. 파이어베이스 Storage관리 객체 얻어오기
                FirebaseStorage firebaseStorage = FirebaseStorage.getInstance("gs://fir-cameratest-ce738.appspot.com");

                // 3. 최상위 노드 참조 객체 얻어오기
                StorageReference rootRef = firebaseStorage.getReference();

                // 4. 읽어오길 원하는 파일의 참조객체 얻어오기
                StorageReference imgRef = rootRef.child("picture.jpeg");

                if(imgRef!=null) {
                    // 5. 참조객체로 부터 이미지의 다운로드 URL을 얻어오기
                    imgRef.getDownloadUrl().addOnSuccessListener(new OnSuccessListener<Uri>() {
                        @Override
                        public void onSuccess(Uri uri) {

                            // 6. 다운로드 URL이 파라미터로 전달되어 온다.
                            Glide.with(getApplicationContext())
                                    .load(uri)
                                    .into(imageview);
                        }
                    })
                            .addOnFailureListener(new OnFailureListener() {
                                @Override
                                public void onFailure(@NonNull Exception e) {
                                    Toast.makeText(getApplicationContext(), "사진이 없습니다.", Toast.LENGTH_SHORT).show();
                                }
                            });
                }
            }
        });
```

## Summary

```
1. Storage에 접근할 수 있는 보안 규칙을 설정하는 것을 확인해야 할 것이다.
2. 다른 포스팅된 곳을 보면 이미지 경로를 "images/test.png" 이러한 경우가 존재하는데
   images는 폴더 파일이니 에러난다고 포기하지 않고 이미지가 루트 폴더에 있다면 단순하게 images만 빼주자
3. Glide는 구글에서 밀고 있는 안드로이드 이미지 로딩 라이브러리이다. 성능이 좋은 로딩 라이브러리로 알려져 있다.
```