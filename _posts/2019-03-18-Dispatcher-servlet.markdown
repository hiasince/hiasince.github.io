---
title:  "Dispatcher-servlet"
date:   2019-03-18 22:13:24
categories: [POST]
tags: [SPRING]
---

# Dispatcher-servlet 이란

Spring은 JAVA 기반의 프레임워크로, Spring MVC는 Spring을 이용하여 웹 어플리케이션을 만들고자 한 모듈이다.
Spring MVC를 이용해서 웹 어플리케이션을 작성하다보면 `Dispatcher-Servlet`이라는 단어를 자주 볼 수 있다.
Dispatcher-Servlet은 빌드과정이든, 로그든 오류든 자주보지만 이에 대한 이해도는 높지 못해서 한 번 알아보고자 한다.

## Servlet

Dispatcher-Servlet을 알기 전에 먼저 Servlet을 알아봐야겠다. Dispatcher-Servlet도 결국 Servlet의 일종이니 말이다.
서블릿(servlet)은 자바언어를 웹어플리케이션에 조금 더 개발하기 쉽게 하기 위해 만든 API(라이브러리, 클래스 들)이며 이 규약에 맞는 라이브러리나 클래스들을 상속 및 구현하여 만든 클래스들을 서블릿이라고 한다.
servlet은 Java코드 안에 HTML태그가 삽입되며 자바언어로 되어있다. HTML 태그를 삽입해서 만드는 서블릿은 웹프로그래밍을 할 수 있지만 자바에 대한 지식이 필요하며 화면 인터페이스 구현에 너무 많은 코드를 필요로 하는 등 비효율적인 측면들이 있다. 때문에 서블릿을 작성하지 않고도 간편하게 웹프로그래밍을 구현하게 만든 기술이 JSP(Java Server Pages)이다. JSP는 서블릿 기반의 스크립트 기술이다. JSP가 컴파일되면 서블릿과 같이 동작하게 되는 것이다.

요즘에는  JSP와 Servlet 동시에 사용하는 MVC모델(View는 JSP, Controller는 Servlet을 사용)을 이용하여 개발하는 형태로 변환되었다. 
이를 통해 프레젠테이션 로직과 비즈니스 로직을 분리할 수 있게되었다.

![1.png](/images/post/Dispatcher-servlet/1.png)

## Servlet Container

위에서 살펴 본 Servlet은 하나의 웹 어플리케이션에서 보면 여러 Controller와 여러 jsp가 존재하듯이 하나만 존재하는 것이 아니기에, 여러 Servlet이 존재하고 이는 Servlet Container에 의해 관리된다. 

서블릿 컨테이너는 이러한 서블릿을 관리하며 네크워크 통신, 서블릿의 생명주기 관리, 스레드 기반의 병렬처리를 대행한다. 즉, 웹 클라이언트로 부터 HTTP 요청이 전달되면 해당 HTTP 요청을 해석하여 적정한 서블릿의 service 메서드를 ServletRequet, ServletResponse 매개변수와 함께 호출한다.

## Dispatcher-servlet

그렇다면 Dispatcher-servlet은 어떤 역할을 하게 되는 것일까?

Spring MVC는 DispatcherServlet이 등장함에 따라 web.xml의 역할을 상당히 축소시켜주었다. 기존에는 모든 서블릿에 대해 URL 매핑을 활용하기 위해서 web.xml에 모두 등록해주어야 했지만, dispatcher-servlet이 해당 어플리케이션으로 들어오는 모든 요청을 핸들링해주면서 작업을 상당히 편리하게 할 수 있게 되었다. 

Dispatcher-servlet의 시퀀스를 보면 Client가 보내는 모든 Request를 잡아 HandlerMapping, Exception, Interceptor 처리를 모두 하고 나서 들어온  Request에 해당하는 Controller에 보내주고, 이후  ViewResolver를 거쳐 View에 렌더링하는 것을 확인할 수 있다.

![2.png](/images/post/Dispatcher-servlet/2.png)

위와 같은 시퀀스를 더 알기 쉽게 우리 말로 풀어쓴 그림이 있어 가져와봤다.

![3.png](/images/post/Dispatcher-servlet/3.png)

결국 Dispatcher-servlet도 수많은 servlet 중의 하나지만, Spring에서 가장 먼저 모든 request를 받고, 최종적으로 reponse를 해주는 역할 맡았다고 볼 수 있다.
## 참고 자료

https://jojoldu.tistory.com/28
http://guruble.com/about-servlet/
https://til0804.tistory.com/25
https://mangkyu.tistory.com/18
https://m.blog.naver.com/PostView.nhn?blogId=acornedu&logNo=221128616501&proxyReferer=https://www.google.com/
