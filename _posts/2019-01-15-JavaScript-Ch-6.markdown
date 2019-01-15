---
title:  "[STUDY] JavaScript Ch 6"
date:   2019-01-15 15:13:24
categories: [STUDY]
tags: [JAVASCRIPT]
---

# 6.1 DOM의 기본 파악하기

브라우저 환경을 전제로 한 내용

## 6.1.1 마크업 언어를 사용하는 표준 방식 'DOM'

클라이언트 측 JavaScript 프로그래밍에서는 HTML을 JavaScript에서 편집한다는 뜻이다.
DOM(Document Object Model)은 마크업 언어로, 작성된 문서에 액세스하기 위한 표준적인 구조로
JavaScript 뿐 아니라 많은 부분에서 사용되고 있다.

## 6.1.2 문서 트리와 노드

DOM은 문서 트리로서 취급한다.
문서에 포함되는 요소, 속성, 텍스트를 각각 객체로 인식하고, 계층구조로 파악한다.

# 6.2 클라이언트 측 JavaScript의 사전지식

클라이언트 측 개발을 하면서 가장 먼저 취해야하는 사항

* 요소 노드의 취득
* 문서 트리 간 왕래
* 이벤트 구동 모델

## 6.2.1 요소 노드 취득하기

* ID값을 키값으로 노드 취득하기 - getElementById 메소드

document.getElementById(id)

* 태그명을 키값으로 노드 취득하기 - getElementsByTagName 메소드

document.getElementsByTagName(name) 복수 형태로 \*를 추가하여 여러 요소 획득 가능
반환 값은 HTML Collection 객체로 length, item(i), namedItem(name) 등을 사용할 수 있다.

*  name 속성을 키로 요소 취득하기 - getElementsByName 메소드

document.getElementsByName(name)

* class 속성을 키로 요소 취득하기 - getElementsByClassName 메소드

document.getElementsByClassName(class)

* 셀렉터 식에 일치하는 요소 취득하기 - querySelector/querySelectorAll 메소드

복잡한 조건으로 검색을 가능하게 하는 방법
document.querySelector(selector)
document.querySelectorAll(selector)

\* 셀렉터 = 모든 요소
#id = 지정한 id 요소
.class = 지정한 클래스명 요소
elem = 지정한 태그명 요소
parent > child = parent요소의 자식요소
...

## 6.2.2 문서 트리 사이 오가기 - 노드 워킹

노드 워킹의 기본은 부모 노드를 선택하여 획득하여, 자식 노드들을 모두 방문하는 것
document의 childNodes 프로퍼티를 이용한다.

```
var s = document.getElementById('food')
var opts = s.childNodes;

for( var  i = 0, len = opts.length ; i< len ; i++) {
}
```
각 노드의 반환된 nodeType에 따라 어떤 노드인지 확인 가능하다.

firstchild/nextSibling 으로도 가능

```
var child= s.firstChild;
while(child) {
child = child.nextSibling;
}
```

## 6.2.3 이벤트를 트리거로 한 처리 실행하기 - 이벤트 구동 모델

브라우저 환경에서는 페이지상에서 이벤트가 발생하고 이에 따른 작동을 코드를 작성한다.
이러한 프로그래밍 모델을 이벤트 드리븐 모델이라고 한다.
이벤트에 대응하여 그 처리 내용을 정의하는 코드를 이벤트 핸들러라고 한다.

* 읽기 = 이미지, 페이지 로딩
* 마우스 = 클릭, 더블클릭, 이동, 올라갔을 시, 벗어났을 시
* 키 = 키를 눌렀을 때, 키에서 떼었을 때
* 폼 = 내용이 변경되었을 때, 리셋 버튼이 눌렸을 때
* 포커스 = 포커스가 벗어났을 때, 포커스 되었을 때
* 스크롤, 사이즈

### 이벤트 핸들러/ 리스너 정의하기

이벤트의 연관성을 부여하는 방법

* 태그 내의 속성으로 선언

```
<input type="button" value="다이얼로그" onclick="btn_click()"/>
<태그명 on이벤트명="JavaScript 코드">
```

* 요소 객체의 프로퍼티로 선언하기
* addEventListener 메소드를 사용하여 선언하기
[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
