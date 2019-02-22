---
title:  "AbstractRoutingDatasource를 이용한 DB 샤딩"
date:   2019-02-21 22:13:24
categories: [POST]
tags: [SPRING]
---

바다TF 데이터 베이스 샤딩 적용기


# 분산 데이터베이스 환경에서 AbstractRoutingDataSource 사용하여 데이터 관리하기


이번 데이터베이스 샤딩을 하는 미션을 수행하기 위해서 많은 구글링을 통해 관리하는 방법들을 모색하기 시작했습니다.
그 중에서 바다TF는 AbstractRoutingDataSource 이용하여, 사용할 DataSource를 라우팅 테이블과 같이 Map 형태로 저장을 해놓고,
들어오는 데이터의 Key값에 따라 적절한 DataSource를 연결해주는!!! 방법을 선택하게 되었습니다.


## AbstractRoutingDataSource 란?


위의 방법을 찾는데 가장 먼저 발견한 키워드는 "AbstractRoutingDataSource" 였습니다. 그런데 과연 이 AbstractRoutingDataSource 는 무엇이고, 어떤 역할을 하게 되는 것인가

AbstractRoutingDataSource는 아래 그림에서 확인할 수 있듯이 InitializingBean을 구체화하고 DataSource를 구체화한 AbstractDataSource를 상속하고 있습니다.
InitializingBean은 빈 객체의 라이프 사이클과 관련된 인터페이스로 bean 객체를 생성하고 프로퍼티를 초기화하고, 설정을 완료한 후 호출되는 메서드를 정의하고 있습니다.
![image](/images/post/AbstractRoutingDataSource/1.png) 
 <br>참조 : http://www.manongjc.com/article/13402.html

## DataSource란?

AbstractDataSource는 DataSource 인터페이스를 구체화한 것으로, JDBC 명세의 일부분이면서 일반화된 연결 팩토리입니다. 
즉 DB와 관계된 connection 정보를 담고 있으며, bean으로 등록하여 인자로 넘겨주게 됩니다. 이 과정을 통해 Spring은 DataSource로 DB와의 연결을 획득하게 되는 것입니다.
![image](/images/post/AbstractRoutingDataSource/2.PNG)<br>
참조: https://gmlwjd9405.github.io/2018/05/15/setting-for-db-programming.html

DataSource는 아래 코드와 같이 DatasourceBuilder를 이용해서 간단하게 생성할 수 있습니다.

```java
public DataSource mailADatasource() {
	return DataSourceBuilder
			.create()
				.url(mailAUrl)
				.username(mailAUserName)
				.password(mailAPassword)
			.build();
}
```

## AbstractRoutingDataSource 구체적 사용법
AbstractRoutingDataSource는 Target이 되는 DataSource를 Map<key(Object), Datasource(Object)> 형태로 가질 수 있고, 원하는 key를 넣게 된다면 key에 맞는 Datasource가 할당되는 형태의 class 입니다.

조금 더 구체적으로 살펴보자면 AbstractRoutingDataSource를 상속받은 class는 determineCurrentLookupKey라는 메소드를 Override하여 어떤 key를 리턴하게하면, 자신이 가지고 있는 TargetDataSource Map에서 지정된 key에 맞는 DataSource 객체를 resolveSpecifiedDataSource 메소드를 통해 결정하게 됩니다.

### 구현상으로 보자면, 먼저 AbstractRoutingDataSource를 상속받는 class를 생성하고, determineCurrentLookupKey 함수를 정의합니다. 

```java
public class MailDataSourceRouter extends AbstractRoutingDataSource {

  @Override
  protected Object determineCurrentLookupKey() {
      return MailDataSourceContextHolder.getMailDatabase();
  }
}
```
### 이 후Configuration을 이용하여, 원하는 Datasource를 생성하여 RoutingDataSource에 targetDataSource Map을 넣어줍니다.
```java
@Configuration
public class DataSourceRoutingConfiguration {

	@Bean
	public DataSource mailDatasource() {
		Map<Object, Object> targetDataSources = new HashMap<>();
		
		DataSource mailADatasource = mailADatasource();
		DataSource mailBDatasource = mailBDatasource();
		
		targetDataSources.put(MailDataSource.MAIL_A, mailADatasource);
		targetDataSources.put(MailDataSource.MAIL_B, mailBDatasource);

		MailDataSourceRouter mailRoutingDatasource = new MailDataSourceRouter();
		
		mailRoutingDatasource.setTargetDataSources(targetDataSources);
		mailRoutingDatasource.setDefaultTargetDataSource(mailADatasource);
		
		return mailRoutingDatasource;
	}
```


여기서 특이한 점은 ContextHolder라는 class를 구현하여 ThreadLocal를 이용하여, Context를 저장한다는 것입니다.
ThreadLocal를 이용하여 Data Access가 여러 DataSource와 transaction을 이용하더라도 신뢰성을 보장할 수 있도록, Thread-safe하게 해주는 것입니다.


## ThreadLocal이란?


Thread는 알겠는데, ThreadLocal은 무엇이고, 뭐가 다를까
ThreadLocal을 Thread-Local 변수를 제공한다. Thread-local Thread 영역에 변수를 설정하는 것으로,
각 Thread 마다 독립적으로 초기화된 복사본을 가지고 있다는 점이 특징입니다.
이러한 특징 때문에 한 쓰레드에서 실행되는 코드가 동일한 객체를 사용할 수 있도록 해 주기 때문에
쓰레드와 관련된 코드에서 파라미터를 사용하지 않고 객체를 전파하기 위한 용도로 주로 사용됩니다.


### 구현 방법도 굉장히 간단하다.


* ThreadLocal 객체를 생성
* ThreadLocal.set() 현재 Thread-Local 변수에 값 저장
* ThreadLocal.get() 현재 Thread-Local 변수 값 조회
* ThreadLocal.remove() 현재 Thread-Local 변수 값 삭제


```java
public class MailDataSourceContextHolder {
	private static ThreadLocal<MailDataSource> CONTEXT = new ThreadLocal<>();

  public static void set(MailDataSource databaseIndex) {
      CONTEXT.set(databaseIndex);
  }

  public static MailDataSource getMailDatabase() {
      return CONTEXT.get();
  }

  public static void clear() {
      CONTEXT.remove();
  }
}
```

저희는 MailDatasource라는 Enum 객체를 생성하여 관리하도록 구현하였습니다.

# Routing Key 설정


위와 같이 설정하게 된다면, 이제 언제 Context의 값을 설정해주고, 무엇을 가지고 설정해줄 것인가에 대한 고민만 남았습니다.
저희 TF는 Key를 설정하는 책임이 Mapper에 있는 것이 좋다고 생각되었습니다.
결국 어떤 DB에 접근하여 SQL문을 실행시킬까에 대한 내용은 Mapper와 밀접하게 연관되어있고
그 결정이 Mapper에서 이루어진다면, 어떤 Service에 의존하지않는 독립적인 Mapper를 작성할 수 있을 것 같았기 때문입니다.<br><br>


> **<span style="color:#000000">그렇다면 Mapper에서 SQL문을 실행시킬 때 Routing Key를 설정하자!!!</span>**

<br>
위와 같이 결심했다면 Aspect를 이용해야한다는 것을 알 수 있었습니다. 물론 각 Mapper에 모든 로직 코드를 삽입하여 해결할 수도 있겠지만, AOP(Aspect Oriented Programming)을 이용한다면 코드의 반복을 줄이고, 효율적으로 관리할 수 있습니다.


## AOP(Aspect Oriented Programming) 개념


AOP(Aspect Oriented Programming)란 관점지향 프로그래밍으로, 핵심 기능들을 분리하여 구현하는 OOP(Object Oriented Programming)에
핵심 기능들에 부가적으로 필요한 로깅, 예외처리, 데이터 검증과 같이 반복되는 부가기능을 따로 분리하여 핵심 기능에 더욱 집중할 수 있고,
부가 기능은 따로 관리하여 반복을 줄이고, 유지보수를 더 편하게 할 수 있도록 관심 분리를 한 것입니다. 이러한 부가기능을 횡단 관심이라고도 합니다.
![image](/images/post/AbstractRoutingDataSource/3.PNG)<br>
참조 : http://closer27.github.io/backend/2017/08/03/spring-aop/

이러한 AOP를 구현하기 위해서는 JoinPoint, PointCut, Advice, Aspect 라는 개념이 추가됩니다.


## JoinPoint


클라이언트가 호출하는 모든 비즈니스 메소드, 모든 메소드를 조인포인트로 생각하면 됩니다.
횡단 관심을 넣을 시점, Joint를 하기 위한 지점이라고 생각하면 됩니다.


## PointCut


포인트컷은 필터링된 조인포인트를 의미합니다. JointPoint 중에 부가적인 처리를 하고 싶은 부분을 필터링하는 역할을 합니다.
포인트컷 지시자를 이용하여 처리를 하고자하는 메소드, 패키지 등을 설정합니다.


* execution () : 가장 강력한 지시자로, 리턴, 타입, 메서드, 패키지 등 조합하여 정교한 PointCut 정의
* within() : 타입 패턴을 이용하여 JoinPoint 정의
* this : 빈 오브젝트 타입에 JoinPoint 정의
* target : 대상 객체 타입 비교를 이용하여 JoinPoint 정의
* args : 메서드의 파라이터 타입을 이용하여 JoinPoint 정의
* @annotation : 특정 어느테이션을 찾는 JoinPoint 정의


## Advice


Adivce는 Aspect의 구현체로 JoinPoint에 삽입되어 동작하는 코드입니다.
이는 횡단 관심에 해당하는 공통 기능의 코드를 의미합니다.


* Before : JoinPoint 전에 실행
* After returning : JoinPoint에서 성곡적으로 리턴한 후 실행
* After throwing : JoinPoint에서 예외가 발생한 경우 실행
* After : JoinPoint의 결과에 상관없이 실행 (finally과 비슷)
* Around : JoinPoint의 전 과정(전,후)에 실행


## Aspect


Aspect는 구현하고자 하는 횡단 관심, 부가 기능을 의미하는 것으로,
한 개 이상의 PointCut(언제 어디서)과 Advice(무엇을)의 조합으로 만들어집니다.


# AOP를 이용하여 Routing Key 설정하기


위의 AOP 개념을 이용하여 Mapper에 정의된 모든 Method가 실행될 때! Routing Key를 설정하고자 하였습니다.
각 Mapper에 Routing Key로 사용할 변수를 찾기 위해 @RoutingKey라는 Annotation을 정의하여 아래와 같이 지정하였습니다.


```java
@Mapper
@RoutingMapper
public interface MailContentsMapper {
	MailContents selectMailContents(@RoutingKey @Param("mailContentsId")String mailContentsId);	//메일 컨텐츠 받아오기
	int insertMail(@RoutingKey @Param("UUID")String mailId, @Param("mail")Mail mail);		// Mail 전송 시 insert
}
```


그 후 Aspect를 생성하여, Around Advice와 execution을 이용하여 Mapper의 어떠한 함수가 실행될 때 수행할 코드를 작성하였습니다.
@RoutingKey로 설정된 Parameter를 찾고 해당 Parameter를 해싱하여 나온 값을 이용해 Context에 등록을 하게된다면, 해당 Key에 맞는
DataSource를 AbstractRoutingDataSource가 결정해줄 것이고, 해당 Datasource에 Mapper가 해당 SQL을 수행할 것 입니다.


```java
@Around("execution(* com.nhnent.rookie.dabada.mapper..*(..))")
public Object aroundTargetMethod(ProceedingJoinPoint thisJoinPoint) {
    MethodSignature methodSignature = (MethodSignature) thisJoinPoint.getSignature();
    Class<?> mapperInterface = methodSignature.getDeclaringType();
    Method method = methodSignature.getMethod();
    Parameter[] parameters = method.getParameters();
    Object[] args = thisJoinPoint.getArgs();

    RoutingMapper routingMapper = mapperInterface.getDeclaredAnnotation(RoutingMapper.class)
    if (routingMapper != null) {
        String Id = findRoutingKey(parameters, args);                           //parameter, args 에서 RoutingKey 어노테이션 찾기
        Integer index = determineRoutingDataSourceIndex(Id);            //얻은 Key를 hash하여 DB 선택할 인덱스 생성
        MailDataSourceContextHolder.set(extractMailDataSoruce(index));
    }
    MailDataSourceContextHolder.clear();
}
```

## 느낀점

의도치 않게 AOP를 먼저 겪어보게 되어 조금 힘들기도 했고, 아직 완벽히 이해는 안되었지만 좋은 경험이라고 생각되어 공유하게 되었습니다.
쓰다보니 생각보다 많이 길어져서 나중에 시간날 때 읽어주시면 좋겠습니다.
감사합니다!


## 참고자료

https://d2.naver.com/helloworld/5812258
https://www.baeldung.com/spring-abstract-routing-data-source
http://closer27.github.io/backend/2017/08/03/spring-aop/
