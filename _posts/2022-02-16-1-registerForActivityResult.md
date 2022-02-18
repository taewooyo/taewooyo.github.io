---
layout: post
title:  "결과 가져오는 registerForActivityResult"
categories: Android
author: bn-tw2020
---
* content
{:toc}

## Activity Result API

- 개발자 앱 내의 액티비트이든 다른 액티비티를 시작하는 것이든 단방향 작업일 필요는 없다.

  다른 액티비티를 시작하고 결과를 다시 받을 수도 있다.

- 예로, 카메라 앱을 시작하고 결과적으로 캡처된 사진을 받을 수 있다.

  사용자가 연락처를 선택하도록 연락처 앱을 시작하고, 결과적으로 연락처 세부 정보를 받을 수 있다.

- 기본적으로 startActivityForResult() 및 onActivityResult() API는 모든 API 수준의 클래스에서 사용하지만,

  AndroidX Activity 와 Fragment에 도입된 Activity Result API 사용을 권장한다.





## 기존 API의 deprecated 이유

- AndroidX Activity, Fragment에 도입된 Activity Result API 사용을 적극 권장하고 있다.

- 결과로 얻은 Activity를 실행하는 로직을 사용할 때, 메모리 부족으로 프로세스와 Activity가 사라질 수 있다.

  Activity에서도 startActivityResult를 통해 콜백 등록 및 onActivityResult에서 콜백처리를 한다.

  두 메서드가 같은 곳에서 구현이 되기에, 메모리 부족으로 동작하지 않을 수 있기 때문이다.

  Activity가 종료되었다가 다시 생성 시에 Activity에게 결과를 기다리는 중임을 알려줘야 한다.


## 새로운 API

- 결과를 얻기 위해 Activity 시작할 때, 메모리 부족으로 프로세스와 Activity가 소멸될 수 있다.

  카메라 앱, 메모리를 많이 사용하는 작업의 경우 소멸 확률이 높다.

- Activity Result API는 다른 Activity을 실행하는 코드 위치에서 결과 콜백을 분리한다.

  Result Callback은 프로세스와 Activity를 다시 생성할 때 사용할 수 있어야 하므로 다른 Activity를

  실행하는 로직이 사용자 입력 또는 비즈니스 로직을 기반으로 발생하더라도 Activity가 생성될 때마다 콜백을 무조건 등록해야 한다.

- registerForActivityResult()는 ActivityResultContract와 ActivityResultCallback을 가져와 

  다른 Activity를 실행하는데 사용하는 ActivityResultLauncher를 반환해야 한다.

- registerForActivityResult()는 콜백을 등록하는 역할을 하며,

  ActivityResultLauncher와 registerForActivityResult()를 사용하면, Activity가 종료되었다가 다시 실행되도

  결과를 기다리고 있다는 것을 알려줄 수 있다.

- requestCode가 없는 이유는 원하는 Activity Request마다 registerForActivityResult를 실행해

  Result Callback을 분리해서 구현함으로 Activity 구분할 필요가 없다.

  ```kotlin
  private lateinit var getResult: ActivityResultLauncher<Intent>
  
  override fun onCreate(savedInstancesState: Bundle?) {
      super.onCreate(savedInstanceState)
      ...
    
      getResult = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) {
          if(it.resultCode == RESULT_OK) {
              ...
          }
      }
      ...
    
      btn.secOnclickListener {
          val intent = Intent(...)
          getResult.launch(intent)
      }
  }
  ```


## Reference

[Android Developers intents result](https://developer.android.com/training/basics/intents/result)  

[registerForActivityResult 상속 관계 파악](https://charlezz.medium.com/%EC%95%A1%ED%8B%B0%EB%B9%84%ED%8B%B0-%EA%B2%B0%EA%B3%BC-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0-good-bye-startactivityforresult-onactivityresult-82bafc50edac)
