---
layout: post
title:  "Toast, Snackbar, Dialog"
categories: Android
author: bn-tw2020
---
* content
{:toc}


## Intro

- 스낵바를 애플리케이션에 적용시키는 과정 속에서 앱 사용자에게 정보를 알릴 수 있는 방법을 정리하게 되었다.

- 앱 사용자에게 정보를 알릴 때, Dialog, Toast, Snackbar을 활용할 수 있다.

  셋 다 정보를 알려주지만, 경우에 따라 사용용도가 다르다.





## Toast

- 토스트는 사용자를 위한 간단한 메시지가 포함된 뷰이다.

- 뷰가 사용자에게 표시되면 애플리케이션 위에 나타나며, 포커스를 받지 않는다.


## Snackbar

- 스낵바는 작업에 대한 간단한 피드백을 제공한다.

- 모바일에서 화면 하단에 간단한 메시지를 표시하고 더 큰 디바이스에는 왼쪽 하단에 표시한다.

- 스낵바는 화면의 다른 모든 요소 위에 나타나며 한 번에 하나만 표시할 수 있다.

- 단일 텍스트 액션에 대해서 사용자와 상호작용을 할 수 있다.
  
  Snackbar는 Dialog보다 사용자에게 주는 영향이 적고, Toast보다 커스텀 하기 편한다.


## Toast vs Snackbar

- Toast는 지정한 일정 시간이 지나야 화면에서 사라진다.

  Toast를 화면에 보여주기 위해서는 Context가 필요하며

  시간 지정을 위해서 Toast.LENGTH_SHORT, Toast.LENGTH_LONG으로 설정이 가능하다.

- Snackbar는 사용자의 입력에 반응하여 사라지게 할 수 있으며, Toast로도 활용할 수 있으다.

  Snackbar는 화면에 보여주려면 View가 필요하며, 지정 가능한 시간은 3가지다.

  Snackbar.LENGTH_SHORT, Snackbar.LENGTH_LONG, Snackbar.LENGTH_INDEFINITE

  Snackbar.LENGTH_INDEFINITE는 사용자 입력이 있을 때까지 화면에 메시지를 보여준다.

  setAction을 활용해 자동으로 사용하지 않고 액션을 기다릴 수 있다.

## Dialog(추가 내용)

- Dialog는 앞으로 이용할 정보를 보여주며, 

  결정을 내리거나 추가 정보를 입력을 요구하는 작은 창으로 이루어져있다.

- Dialog는 정보와 2가지 action을 제공할 수 있다.

- Dialog는 방해하는 속성이 존재하기 때문에 사용자가 하던 일을 멈추고 Dialog 처리가 우선시 된다.

- 재난재해가 일어날 때 긴급 메시지가 오듯이, 즉각적인 응답이 필요한 경우 Dialog가 적절하다.


## Reference

[Android Developers Toast](https://developer.android.com/reference/android/widget/Toast.html)  

[Android Developers Snackbar](https://developer.android.com/reference/android/support/design/widget/Snackbar.html)  

[Android Developers Dialog](https://developer.android.com/reference/android/app/Dialog.html)  


  