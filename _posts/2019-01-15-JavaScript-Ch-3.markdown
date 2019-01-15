---
title:  "[STUDY] JavaScript Ch 3"
date:   2019-01-15 15:13:24
categories: [STUDY]
tags: [JAVASCRIPT]
---

# 3.1 객체란?

객체 지향적 언어다

## 3.1.1 객체 = 프로퍼티 + 메소드

* 프로퍼티 = 개체의 상태나 특성을 나타내는 정보 (변수)
* 메소드 = 개체를 조작하기 위한 도구 (함수)

## 3.1.2 객체를 이용하기 위한 준비 - new 연산자

C++과 동일하나, 기본적으로 var 변수명 = new 객체명([인수]);로 생성자 호출가능

## 3.1.3 정적 프로퍼티/ 정적 메소드

정적 프로퍼티/정적 메소드, 클래스 프로퍼티/클래스 메소드란 인스턴스를 생성하지 않고 바로 이용

## 3.1.4 내장형 객체란?

기본적으로 제공되는 객체들
기본적으로 제공되는 객체들에게는 new 연산자를 쓸 필요가 없다.

# 3.2 기본 데이터를 취급하기 위한 객체

표준 데이터를 취급하기 위한 String/Number/Symbol 객체

## 3.2.1 문자열 조작하기 - String 객체

C++과 동일

## 3.2.2 숫자 조작하기 - Number 객체

산술 연산자에 의한 문자열 숫자 변환 가능

## 3.2.3 심벌 작성하기 - Symbol 객체

var symbo = Symbol(['sym'])

* 개별의 것은 별개의 것으로 간주
* 형변환 불가

## 3.2.4 기본적인 숫자 연산 실행하기 - Math 객체

수학연산 기능을 제공하는 객체

# 3.3 값의 집합을 처리/조작하기 - Array/Map/Set 객체

## 3.3.1 배열 조작하기 - Array 객체

스택 - push/pop/shift/unshift
큐 - push/pop/shift/unshift
splice = 해당 구간 요소 변경

* foreach = 배열의 개별 요소에 처리하기 위한 함수
foreach(function(value, index, array) {
})
* some
* filter
* sort

## 3.3.2 연상 배열 조작하기 - Map 객체

## 3.3.3 중복되지 않은 값의 집합 조작하기 - Set 객체

# 3.4 날짜/시간 데이터 조작하기

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
