---
title:  "Java Class Load 과정 - Java 8"
date:   2020-02-16 22:13:24
categories: [POST]
tags: [JAVA]
---

Java로 프로그래밍을 하는 사람이라면 우리가 .java 파일로 작성한 소스 코드가 빌드하면서 어떤 과정을 거치는지, 어떻게 JVM에 올라가는지 아는 것은 중요합니다. 그래야 클래스 로딩 때 발생하는 이슈를 해결할 수 있고 코드 상에서 동적으로 클래스를 로딩하는 구문을 이해할 수 있으며, Java의 동작 방식에 대해 한층 깊게 알게되는 것이기 때문입니다.

## Java의 Build 과정

"Build"는 우리가 작성한 코드를 실행 가능한 형태로 만드는 작업을 말합니다. Java에서는 build하기 위해 아래의 과정을 거치게 됩니다.

* 생성할 코드가 있는 경우 코드 생성 (Lombok, Object Mapping ... )
* 소스 코드 컴파일 (.java -> .class 확장자 변경, cmd에서 javac 명령어와 같은 동작)
* 테스트 코드 컴파일
* 테스트 실행
* 패키징 (jar, war ...)

위와 같은 과정을 거치게 되면 실행가능한 JAR(Java Archive), WAR(Web application Archive)와 같은 파일이 생성됩니다. jar와 같은 패키징 파일은 우리가 코드 작성 시 여러 class를 작성하기 때문에 컴파일을 하게 되면 여러 class가 나오게 되고, 하나라도 없으면 안되기 때문에 하나로 묶는 패키징 작업이 필요한 것입니다. 아무튼 Java의 Class Loader는 이제 이 class들을 JVM에 로드하는 역할을 수행하게 됩니다.

## Java Class Loader

Java에서 클래스가 로딩 과정은 Class Loader가 필요한 .class 클래스 파일들을 찾아 그것을 JVM위에 올려놓는 과정을 뜻합니다.

Java의 class loader는 3가지 클래스 로더가 존재합니다. 왜 3가지 클래스 로더가 존재하고, 어떤 클래스들을 각각 로드하는 지 간단한 예제를 통해 알아볼 수 있었습니다. (예제 출처 : https://www.baeldung.com/java-classloaders)

```java
public void printClassLoaders() throws ClassNotFoundException {
    System.out.println("Classloader of ArrayList:"
        + ArrayList.class.getClassLoader());
 
    System.out.println("Classloader of Logging:"
        + Logging.class.getClassLoader());
 
    System.out.println("Classloader of this class:"
        + PrintClassLoader.class.getClassLoader());
 
}
```

위의 코드를 실행하게 된다면, 결과는 아래와 같습니다.

```
Class loader of ArrayList:null
Class loader of Logging:sun.misc.Launcher$ExtClassLoader@3caeaf62
Class loader of this class:sun.misc.Launcher$AppClassLoader@18b4aac2
```

결과에서 볼 수 있듯이 Java에서는 3가지 기본 클래스 로더가 존재하는데, 이는 Bootstrap Class Loader, Extension Class Loader, Application/System Class Loader 입니다.

### Bootstrap Class Loader

첫 번째는 Bootstrap Class Loader로 3가지 기본 클래스로더 중 최상위 클래스로더입니다. 클래스 로더들도 결국 Java의 클래스로 최 상위 클래스 로더인 Bootstrap Class Loader에 의해 로드됩니다. 이는 주로 JDK 내부 클래스, 일반적으로 `jre/lib/rt.jar`에 담긴 JVM을 실행시키기 위한 핵심 클래스들을 로딩합니다.(rt는 RunTime를 의미) 그런데 위에서 JDK에 있어야하는 ArrayList의 클래스 로더는 null이 나왔었는데, 이러한 이유는 Bootstrap Class Loader는 자바가 아닌 네이티브 코드로 작성되어 있어서 자바 클래스로 표시되지 않기 때문입니다. 

###Extension Class Loader 

두 번째는 Extension Class Loader로 Java의 확장 클래스들을 로더해주는 역할을 합니다. 주로  *$JAVA_HOME/lib/ext* 에 위치해 있는 클래스들을 로드합니다. Java가 기본적으로 제공하는 클래스도 아니고, 내가 직접 작성하지도 않은 외부 라이브러리들을 가져올 때, 해당 위치에 jar를 넣게되고, Extension Class Loader가 거기에 있는 클래스를 로드해줍니다.

### Application Class Loader

마지막으로는 `Application/System Loader`로 classpath나 실행시킨 jar 파일 안에 있는 manifest 파일에 지정된 경로에 있는 클래스들을 로딩합니다. 쉽게 말해 내가 작성한 클래스를 로드해주는 역할을 합니다.

![image](/images/post/JavaClassLoader/1.png)

출처 : https://dev.vividbreeze.com/jvm-classloading/

각 클래스 로더들은 설명에서 느껴지듯이 계층적으로 동작합니다. Bootstrap Class Loader가 기본적으로 필요한 클래스들을 로드하고, Extension Class Loader가 외부에서 가져온 클래스들을 로드하고 마지막으로 Application Class Loader가 실행시킨 파일에 정의된 클래스들을 로드하는 과정으로 진행됩니다. 그러면 클래스 로더의 종류와 각각의 역할은 알았으니, 실제로 해당 클래스 로더들이 어떤 방법으로 클래스를 로드하는지 알아보겠습니다.

## Java Class Load 과정

클래스 로더는 Java Runtime Environment의 일부분입니다. JVM이 클래스를 요청하면 클래스 로더는 클래스 이름을 사용하여 클래스를 찾고 클래스의 정의를 런타임에 로드하려고 시도하게 됩니다. Java.Lang.ClassLoader.loadClass() 메소드를 이용하여 해당 클래스가 있는지 찾고, 만약 찾지 못한다면 상위 클래스 로더로 위임하게 됩니다. 결국 부모도 못찾게되면, java.net.URLClassLoader.findClass() 메소드를 이용하여 파일 시스템에서 찾게 됩니다. 이 마저도 실패하면 ClassNotFoundException 또는 NoClassDefFoundError를 throw하게 됩니다. 위의 과정은 3가지 원칙을 준수하면서 진행하게 됩니다.

### Delegation Model (위임 모델)

Delegation Model(위임 모델)은 **하위 클래스 로더는 클래스 또는 리소스 검색을 상위 클래스 로더에 위임한다**는 원칙입니다. 그래서 최상위 클래스 로더인 Bootstrap ClassLoader 부터 자신이 해당 클래스를 가지고 있는지 확인한 후 결과를 돌려주는 방식으로 진행됩니다.

![image](/images/post/JavaClassLoader/2.png)

출처 : https://homoefficio.github.io/2018/10/13/Java-클래스로더-훑어보기

`ClassLoaderRunner`는 자기 자신을 로딩한 Application Class Loader에게 Internal 클래스 로딩을 요청합니다. 클래스 로딩 요청을 받은 Application Class Loader는 스스로 직접 로딩하지 않고 상위 클래스로더인 Extension Class Loader에게 위임하게 됩니다. Extension Class Loader는 최상위 클래스 로더인 Bootstrap Class Loader에게 다시 위임하게 되고, 최상위 클래스 로더에 도달하면 rt.jar에 있는지 확인하고 있으면 반환, 없으면 다시 하위 클래스로 보내는 로직을 반복하게 되고, 마지막에도 못찾게되면 ClassNotFoundException을 throw하게 됩니다. 

### Visibility Principle

Visibility Principle(가시성 원칙)은 **하위 클래스로더는 상위 클래스로더가 로딩한 클래스를 볼 수 있지만, 상위 클래스로더는 하위 클래스로더가 로딩한 클래스를 볼 수 없다**는 원칙입니다. 예를 들자면, Application Class Loader는 Extension Class Loader, Bootstrap Class Loader가 로딩한 클래스는 볼 수 있으나, 반대로 Extension Class Loader, Bootstrap Class Loader는 Application Class Loader가 로딩한 클래스는 볼 수 없다는 원칙입니다. 3가지 클래스 로더가 가지는 정체성이 분명하기에 이러한 원칙을 지켜야하는 것으로 보입니다.

### Unique Classes

Unique Classes(유일성 원칙)은 **하위 클래스로더는 상위 클래스로더가 로딩한 클래스를 다시 로딩하지 않게 해서 로딩된 클래스의 유일성을 보장**하는 것입니다. 



### 참고

https://engkimbs.tistory.com/606

http://www.itworld.co.kr/news/110768

https://www.baeldung.com/java-classloaders

https://homoefficio.github.io/2018/10/13/Java-클래스로더-훑어보기
