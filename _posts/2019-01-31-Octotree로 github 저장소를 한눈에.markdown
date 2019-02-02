---
title:  "Octotree로 github 저장소를 한눈에"
date:   2019-02-02 15:13:24
categories: [POST]
tags: [GIT]
---

github repository를 웹에서 확인하고플 때가 종종 있다. 이럴 때 디렉토리 구조가 복잡한 경우에는 확인하기 매우 힘들다...
이러한 불편함을 해소하고자, Octotree라는 것을 소개하고자 합니다. 소개해주신 정명주 책임님 감사합니다!
Octotree는 GitHub 저장소의 디렉토리를 한 눈에 파악할 수 있게 해줍니다. 
웹의 왼쪽 사이드바에서 트리 형식으로 저장소의 디렉토리 구조를 보여준도록 해주는 플러그 인!!

적용 과정도 매우 심플합니다.

## 1.크롬 웹스토어에서 Octotree를 다운로드 받는다.
![image](/images/post/octotree/octotree1.JPG)
## 2. token을 만드는 Setting으로 간다.
![image](/images/post/octotree/octotree2.JPG) ![image](/images/post/octotree/octotree3.JPG)
github에 들어가서 우측 상단의 프로필을 눌러 Setting을 선택하고, 그 중에서도 개발자 Setting을 선택한다.

## 3. 그 중에 personal token을 받는다.
![image](/images/post/octotree/octotree4.JPG)
token을 받기전에 repo에 모두 선택한 후 받는다.
![image](/images/post/octotree/octotree5.JPG)
이 때 받은 token은 다시 돌아왔을 때는 보이지 않으니 얼른 복사해야한다!!

## 4. 만든 token를 등록하자.
![image](/images/post/octotree/octotree6.JPG)
이후에 git repository에 들어가면 좌측 상단에 화살표가 뜨고 여기에 복사했던 토큰을 넣으면 끄읕
![image](/images/post/octotree/octotree7.JPG)

Octotree를 사용하고 난 후기는, 굉장히 편하다. 트리구조로 나오기 때문에 마치 IDE로 둘러보는 느낌이 날 뿐더러 간단히 뒤로가기로 움직이던 시절을 생각하면
답답함에 숨이 막혀온다.
