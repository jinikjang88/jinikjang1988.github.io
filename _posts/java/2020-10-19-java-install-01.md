---
layout: post
title: "Java JDK11 설치하기"
subheading: JDK11 설치하기
author: Jeffrey
categories: Java
banner: "/assets/images/banners/java.jpeg"
tags: Java
---


# JDK 11 설치 및 설정 하기 (Windows OS)    

JDK 란? (Java Development Kit)  
- JDK는 Java 프로그래밍 언어를 사용하여 응용 프로그램 및 구성 요소를 개발하기 위한 개발 환경이다.  
- JDK에는 Java 프로그래밍 언어로 작성되고 Java 플랫폼에서 실행되는 프로그램을 개발, 테스트 및 모니터링하는 데 유용한 도구가 포함되어 있다.  

## 설치절차
    1. Oracle 가입하기
    2. JDK 11 Download 하기
    3. Windows 환경 변수 설정하기
    4. 설치 확인 및 HelloWorld


### 1. Oracle 가입하기
먼저 아래의 Oracle 계정 만들기 사이트에 접속하여 안내에 따라 필수 입력사항들을 작성하고, 계정을 만들어보자.  
[https://profile.oracle.com/myprofile/account/create-account.jspx](https://profile.oracle.com/myprofile/account/create-account.jspx)

### 2. JDK 11 Download 하기
가입이 완료되었다면 이제 JDK 11을 아래 사이트에서 (Windows x64 Compressed Archive) 항목을 다운로드 하자.  
[https://www.oracle.com/java/technologies/javase-jdk11-downloads.html](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)

![java-01-00](../../../../assets/images/post/java_01/00.png)
  

JDK 버전이 다양하게 있지만 버전 'JDK 11'을 받는 이유는 'JDK 11'이 LTS 버전이기 때문에 설치 한다.    
LTS란 'Long Term Support'의 약자로 장기지원버전이기 때문이다.  
간략히 말하면 안정된 버전이다.  

다운로드 위치는 고급 개발자에 따라 달라질 수 있으나, 통상 기본적으로 C드라이브의 Program Files의 Java 디렉토리를 만들어 압축을 풀도록 하자  

    C:\Program Files\Java\jdk-11.0.8  


### 3. Windows 환경 변수 설정하기  
이제 윈도우 환경에서 Java를 실행할 수 있도록 환경 변수들을 설정하도록 하자.  

3.1 제어판 > 시스템 > '고급 시스템 설정' > 고급 탭 > 환경번수 클릭  

![java-01-01](../../../../assets/images/post/java_01/01.png)
![java-01-02](../../../../assets/images/post/java_01/02.png)

3.2 시스템 변수 항목의 Path 설정 값을 아래와 같이 추가하자   
![java-01-03](../../../../assets/images/post/java_01/03.png)
![java-01-04](../../../../assets/images/post/java_01/04.png)


### 4. 설치 확인 및 HelloWorld 

4.1 Windows CMD(Command) 실행  

왼쪽 아래의 윈도우 로고 클릭 > cmd 검색 > 실행  
![java-01-05](../../../../assets/images/post/java_01/05.png)


4.2 Java 버전 확인  
```
java -version
```

  
![java-01-06](../../../../assets/images/post/java_01/06.png)


4.3 Java Compile 명령어 확인
    
```shell script
javac
```
  
![java-01-07](../../../../assets/images/post/java_01/07.png)  


4.4 Java 실행 명령어 확인  
  
```shell script
java
```  
  
![java-01-08](../../../../assets/images/post/java_01/08.png)  
 
  
4.5 Hello World  
  
```shell script
notepad HelloWorld.java
```
  

텍스트 편집 창에 다음과 같이 코딩한다.  

```java
class HelloWorld {

  public static void main(String []args) {

    System.out.println("Hello World ~ ");

  }

}

```

저장 후 해당 파일을 컴파일 해보자.  
컴파일이 완성이 되면 해당 디렉토리에 HelloWorld.class 파일이 생성된다.  
  
```shell script
javac ./HelloWorld.java
```

이제 처음으로 만들어본 프로그램을 실행해보자.  
실행하면 아래의 그림과 같이 Hello World ~ 문구가 출력이 된다.  
    
```shell script
java HelloWorld
```

![java-01-09](../../../../assets/images/post/java_01/09.png)  