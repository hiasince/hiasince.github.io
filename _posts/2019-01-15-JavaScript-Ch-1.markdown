---
title:  "[STUDY] JavaScript Ch 1"
date:   2019-01-15 15:13:24
categories: [STUDY]
tags: [JAVASCRIPT]
---

# 1-1 JavaScript란

JavaScript란 Netscape Communication이 개발한 브라우저 전용의 스크립트 언어.
Java와 비슷하여 JavaScript라 불리우지만, 전혀 다른 언어이며 호환성도 없다.

## 1.1.1 JavaScirpt의 역사

**JavaScirpt 전성기**

1990년대 후반 = JavaScirpt의 전성기 (Fade In/Out, 문자열 점멸 등 시각적인 효과가 JavaScirpt에 의해 실현되는 시기)
JavaScirpt는 페이지의 시각적인 부분, 사용상의 편리성 향상을 위해 사용되어짐.

**JavaScirpt 암흑기**

이 후  과도한 웹페이지 치장으로 인해 안 좋은 웹페이지 대량 양산으로 안 좋은 이미지 확산
브라우저 밴더가 많아지면서 크로스 브라우저 문제가 커짐
JavaScirpt의 내부 실정과 관련된 브라우저의시큐리티 홀 문제 발생

```
JavaScirpt Security hole

JavaScript가 DOM과 상호 작용하는 방식은 악의적인 행위자들이 웹을 통해 임의적으로 스크립트를 삽입하여 클라이언트 컴퓨터에서 실행할 수 있게 함으로써 클라이언트에게 위험을 준다.

이러한 취약점을 막기 위한 방법이 몇 가지 존재한다.

1. 샌드박스(Sandboxing), 스크립트 독자적 실행

샌드박스 또는 스크립트를 별도로 실행하여 특정 리소스에만 액세스하고 작업을 수행하도록 함

2. 동일한 정책을 구현

어떠한 스크립트가 다른 사이트의 스크립트에 사용되는 데이터 액세스를 방지하는 방법

```

## 1.1.2 JavaScirpt 부활 - Ajax, 그리고 HTML5의 시대


2005년 2월 **Ajax(Asynchronous JavaScirpt + XML)** 기술 등장 후 부활

### Ajax란 브라우저 상에서 데스크톱 애플리케이션과 같은 웹 페이지를 작성하기 위한 기술!

브라우저 표준 기술인 HTML, CSS, JavaScirpt 만으로 많은 컨텐츠를 만들 수 있어졌다.
HTML5 등장 후 JavaScirpt API 강화하여 더욱 좋아짐
스마트폰/태블릿 PC의 보급, SPA(Single Page Application) 유행으로 인해 브라우저만으로 실행 가능한 JavaScirpt가 각광

```
SPA 란? 단일 페이지로 구성된 웹 애플리케이션을 말한다.

Ajax의 등장으로 비동기 통신을 통해  JavaScirpt만으로 처리 못하는 부분을 해결하여 구현한다.
```
## 1.1.3 나쁜 이미지에 대한 오해

JavaScirpt는 초급자 언어이다?

->  JavaScirpt도 Java, C++과 같은 객체지향 언어이다.

JavaScirpt에 시큐리티 홀이 많다?

->  JavaScirpt를 장착한 브라우저의 문제, 요즘에는 브라우저 벤더의 시큐리티 의식이 높아져 많이 없어진 상태

JavaScirpt는 크로스 브라우저 문제가 있어 생산성이 떨어진다

->  표준화를 통해 호환성이 증가했다.

## 1.1.4 언어로서의 네 가지 특징

### 스크립트 언어이다
단 기간에 배울 수 있는 장점이 있고, 객체지향적인 구조도 가져 재사용성 유지보수에 좋다.
### 인터프리터 언어이다
라인 바이 라인으로 해석, 컴파일이 없어서 바로 실행 가능
### 여러 환경에서 이용 가능하다
JavaScirpt를 기반으로 만든 언어들이 실제 여러 환경에서 동작 (node.js, android, ios(webview) 등등)
### 몇 개의 부분으로 구성되어 있다
CoreJavaScirpt 표준적인 기능을 제공하는 부분, DOM(Document Object Model) 도큐먼트를 객체적으로 조작하게 해주는 구조

# 1-2 ECMAScript 2015란?
ECMAScript란 표준화 단체인 ECMA가 제정한 JavaScirpt이다.

ES2015에서 제공되는 사항
* class 명령 도입으로 class 정의 가능
* import/export 명령으로 모듈화 지원
* 함수 구문 개선
* let/const 스코프 도입
* 반복자/생성자에 의한 객체 조작 가능
* String/Number/Array 등 내장형 객체 기능 확장

## 1.2.1 브라우저별 대응 상황
Firefox, Chorme, Edge는 90% 이상 대응률, Exploer11 20%, Safari 50%
대응률이 낮은 브라우저에서는 트랜스 컴파일러를 통해 ES2015코드를 ES5 사양의 코드로 변환하는 일이 필요

# 1-3 브라우저의 개발자 도구
브라우저에 표준으로 탑재되어있는 개발자 도구는 JavaScirpt/CSS 개발에 반드시 필요

## Elements
**Elements 탭에서 HTML을 트리구조로 표시**

JavaScirpt로 동적으로 조작한 결과가 반영되어있어 스크립트 실행결과를 확인할 때 유효
즉시 결과가 반영되므로 스타일을 조절하는데도 유용하다.
## Network
**Network 탭에서 브라우저상에서 발생한 통신을 확인할 수 있다.**

특히 Ajax로 발생한 비동기 통신은 표로 나타내기 어려움으로 네트워크 탭을 이용하여 올바른 요청, 올바른 데이터를 수신하는지 확인
비동기 통신에서 Header, Response를 확인하는 경우가 많다

## Source
**Source 탭에서 브레이크 포인트를 설정하여 디버그가 가능하다.**

디버그 시 중단점에 걸리는 경우 Watch를 통해 변수 상태 확인 가능하다. 스탭인, 스탭아웃 등을 이용해 더욱 유용하게 사용 가능하다.

## Application

**Application탭에서 스토리지/쿠키를 확인하고, 값을 추가/편집/삭제도 가능하다.**

## Console

 **Console 탭에서 오류 메세지나 로그 확인을 하거나, 대화적으로 JavaScirpt 코드를 실행가능해 디버그에 많은 도움을 준다**

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
