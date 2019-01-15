---
title:  "[STUDY] JavaScript Ch 5"
date:   2019-01-15 15:13:24
categories: [STUDY]
tags: [JAVASCRIPT]
---

[STUDY] JAVASCRIPT MASTER BOOK - 기초부터 실무 응용까지 CHAPTER 5
루키바다TF - 한병익

# 5.1 JavaScript의 객체지향 특징

## 5.1.1 '클래스'는 없고 '프로토타입'만 있다

프로토타입이란 속박이 약한 클래스로 생각

## 5.1.2 가장 간단한 클래스 정의하기

JavaScript에서는 함수에 클래스 역할을 부여한다.

## 5.1.3 생성자로 초기화하기

new를 이용해서 초기화하는 것.
this개념은 C++과 동일

## 5.1.4 동적으로 메소드 추가하기

인스턴스화 해버린 객체에 대해서 나중에 메소드를 추가할 수 있다.
그러나 동일한 클래스로 인스턴스화 하여도, 추가한 메소드에 대해서 공유하지는 않는다.

## 5.1.5 문맥에 따라 내용이 변하는 변수 - this 키워드

this가 참조하는 곳

* 톱 레벨 = 글로벌 객체
* 함수 = 글로벌 객체 (Strict에서는 undefined)
* call/apply 메소드 = 인수로 지정된 객체
* 이벤트 리스너 = 이벤트 발생처
* 생성자 = 인스턴스
* 메소드 = 호출원의 객체

call/apply 메소드는 함수가 제공하는 멤버로 함수를 호출한다.

## 5.1.6 생성자의 강제적인 호출

함수가 생성자 역할을 담당하고 있기 때문에 생성자를 함수로서 호출이 가능하다는 문제점 존재
```
if(!(this instanceof Member)) {
 return new Member();
}
```
위와 같은 코드로 방지

# 5.2 생성자의 문제점과 프로토타입

공통의 메소드를 정의하기 위해서는 생성자에서 정의, 그러나 쓸데없이 메모리 소비가 크다.
그렇다고 메소드를 복사하는 것도 쓸데 없다.

## 5.2.1 메소드는 프로토타입으로 선언한다 - prototype 프로퍼티

prototype이라는 프로퍼티에 대입된 멤버는 인스턴스 된 객체에 인계된다.
베이스가 되는 객체에 속하는 프로트타입 객체에 대해 암묵적인 참조를 가지게 된다.

## 5.2.2 프로토타입 객체를 사용한 메소드 정의의 두 가지 이점

* 메모리의 사용량을 절감할 수 있다.
* 멤버의 추가나 변경을 인스턴스가 실시간으로 인식할 수 있다.

## 5.2.3 프로토타입 객체의 불가사의(1) - 프로퍼티 설정

프로토타입을 통해 프로퍼티를 설정하는 경우에 모든 인스턴스에 영향을 미치는가?
프로토타입으로 설정되어 있어도, 인스턴스가 따로 설정한 경우 인스턴스가 가지는 것이 더 우선순위를 가진다.
C++의 Derived와 비슷한 개념

## 5.2.4 프로토타입 객체의 불가사의(2) - 프로퍼티의 삭제

삭제하는 경우에도 프로토타입으로 설정되어 있어도, 인스턴스가 따로 설정한 경우 인스턴스가 가지는 것이 더 우선순위를 가진다.
C++의 Derived와 비슷한 개념

## 5.2.5 객체 리터럴로 프로토 타입 정의하기

```
.prototype =  {
 getName : {
 },
 toString : {
 }
};
```
위와 같은 객체 리터럴로 프로토타입을 정의할 수 있다.

# 5.3 객체의 계승 - 프로토타입 체인

계승(상속)에 있어서 슈퍼클래스와 서브 클래스가 존재

## 5.3.1 프로토타입 체인의 기초

C++의 상속과 동일
슈퍼 클래스의 생성자 호출하는 법
Animal.call(this, '인수들');

## 5.3.2 계승 관계는 동적으로 변경 가능

계승한 결과는 인스턴스가 생성된 시점에서 고정된다.

## 5.3.3 객체의 타입 판정하기

* 바탕이 되는 생성자 취득하기 - constructor 프로퍼티

상속이 되는 경우 생성자는 슈퍼클래스를 따라간다.

* 바탕이 되는 생성자 판정하기 - instanceof 연산자

instanceof 연산자의 경우에는 슈퍼클래스, 서브클래스 모두 인정

* 참조하고 있는 프로토타입 확인하기 - isPrototypeOf 메소드

isPrototypeOf 메소드의 경우에는 슈퍼클래스, 서브클래스 모두 인정

* 멤버의 유무 판정하기 - in 연산자

특정 멤버내 존재하는 지 판단하기 위한 연산자

# 5.4 본격적인 개발에 대비하기

## 5.4.1 private 멤버 정의하기

private를 정의하기위한 구문은 없으나, 클로저를 이용함으로써 유사적으로 구현 가능

```
var _checkArgs = function(val) {
	return (typeof val === 'number' && val > 0);
}

if(_checkArgs(base)) { _base = base;}
```

privileged 메소드를 통해 private 멤버에 액세스
Access, getter, setter를 통해서 접근할 것

## 5.4.2 Object.defineProperty 메소드에 의한 액세서 메소드 구현

Object.defineProperty(obj, prop, desc)
```
Object.defineProperty(this,
'base', {
get: {}
set: {}
});
```

위와 같이 간단히 get, set을 구현, 여러 프로퍼티도 가능

## 5.4.3 네임스페이스/패키지 작성하기

C++에 있는 네임스페이스를 객체를 통해 구현

# 5.5 ES2015의 객체지향 구문

## 5.5.1 클래스 정의하기 class 명령

class 명령을 사용하여 정의 가능해졌음
class와 function 생성자와는 다르다. 항상 선언을해야하며 new를 이용하여 생성해야한다.
계승을 위해서 extends 구문을 사용한다. super를 통해 상위 객체 호출

## 5.5.2 객체 리터럴 개선

별거 아님

## 5.5.3 애플리케이션을 기능 단위로 모으기 - 모듈

기능 단위로 분할/정리하는 모듈의 존재를 생성
모듈을 파일로 정의 하였을 때 export를 통해서 액서스가 가능한 클래스를 정한다
외부에서 import { 클래스명, ..} from '파일 경로' 방법을 통해 가져온다.
import 시 as를 이용하여 별명 설정 가능 (like python)

## 5.5.4 열거 가능한 객체 정의하기 - 반복자

iterator를 이용하여 for of 명령으로 내부 요소를 열거할 수 있다.
```
let data_ary = [1,2,3];
let itr = data_ary[Symbol.iterator]();
let d;

while(d = iter.next()){
 if(d.done) {break;}
}
```

## 5.5.5 열거 가능한 객체를 더욱 간단하게 구현하기 - 발생자

Generator를 이요하여 열거가능한 객체를 더욱 쉽게 구현한다.

```
function* mG() {
 yield 'asd';
 yield 'azd';
}
```
yield를 이용하여 여러 번 반환하는 것과 같은 효과를 보여줌

## 5.5.6 객체의 기본적인 동작을 사용자 정의하기 - Proxy 객체

Proxy를 이용하여 객체를 조작한 타이밍에 로그를 출력하는 등 어떤 프로퍼티를 얻을 때 부수적 처리를 한다.
cf) explorer/safari에서 지원 불가
new Proxy(target, handler);


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
