---
layout: post
title: "웹 동작원리"
categories: 네트워크
author: bn-tw2020
---
* content
{:toc}

## Intro

```
사용자가 주소창에 www.naver.com라고 검색하면 동작 원리를 정리하는 글입니다.
```




---

## 동작 과정

```
1. PC는 DNS 서버의 IP주소가 설정되어야 한다.
2. PC는 DHCP 프로토콜로 IP 주소를 할당 받으면서 DNS 서버 IP주소를 DHCP를 통해 함께 받는다.
   (보통 2개의 DNS IP를 받는다. Primary DNS 서버가 죽었을 때 Secondary DNS 서버에 물어본다.)

사용자가 "www.naver.com" 를 입력했을 때 경우

1. PC는 미리 설정되어 있는 DNS 서버(local DNS)에게 "www.naver.com" 라는 hostname에 대한 IP주소를 물어본다.

2. local DNS에는 "www.naver.com" IP 주소가 있으면, 그것을 이용하며,
                                   주소가 없다면, 다른 DNS서버로부터 IP 주소를 찾는다.

3. local DNS는 Root DNS 서버에게 "www.naver.com"를 물어본다.
   local DNS은 Root DNS 서버의 IP 주소 어떻게 알지? 라는 의문을 갖게 된다.
   어떻게 알고 있는지는 Reference 두번째 블로그를 참고하면 된다.

4. Root DNS는 전세계에 13대(미국 10대, 일본, 네덜란드, 노르웨이)가 있으며,
   우리 나라는 Root DNS 서버에 대한 미러 서버를 3대 운용 하고 있다.

5. Root DNS 서버는 "www.naver.com"의 IP 주소를 모른다.
   local DNS 서버에게 "난 www.naver.com에 대한 IP 주소 몰라.
   나 말고 내가 알려주는 다른 DNS 서버에게 물어봐" 라고 응답을 합니다.

6. 다른 DNS 서버는 com 도메인을 관리하는 DNS 서버다.

7. local DNS는 com 도메인을 관리하는 DNS 서버에게 "www.naver.com" IP주소를 물어본다.

8. com 도메인을 관리하는 DNS 서버에도 해당 정보가 없다.
   그래서 이 DNS 서버는 Local DNS 서버에게 "난 www.naver.com에 대한 IP 주소 몰라.
   나 말고 내가 알려주는 다른 DNS 서버에게 물어봐!"라고 응답을 합니다.
   이 다른 DNS 서버는 naver.com 도메인을 관리하는 DNS 서버이다.

9. local DNS는 다시 "naver.com 도메인을 관리하는 DNS 서버에게 "www.naver.com" IP주소를 물어본다.

10. naver.com 도메인을 관리하는 DNS 서버에는 "www.naver.com" hostname에 대한 IP주소가 있다.
    local DNS 서버에게 "www.naver.com" IP주소를 응답한다.

11. local DNS는 "www.naver.com"에 대한 IP주소를 캐싱을 하고 이후 다시 이용할 수 있습니다.
```

* 위에서 Local DNS 서버가 여러 DNS 서버를 차례대로(Root DNS -> com DNS -> naver.com DNS)
  물어봐서 답을 얻는 과정을 Recursive Query라고 부른다.

* 참고
   * https://www.naver.com/index.html : URL
   * www.naver.com : Host Name
   * .com : Top-level Domain Name
   * .naver.com : Second-level Domain Name

## Reference

[DNS정리글](http://blog.naver.com/PostView.nhn?blogId=kostry&logNo=220899042020)

[DNS Query](https://webdir.tistory.com/161)  

[네트워크 동작원리 동영상 링크](https://www.youtube.com/watch?v=oW_EirDkCnM)