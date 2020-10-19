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

어떤 일을 하기에 앞서 우리는 질문을 먼저 던져 본다.  

우리는 라면이 먹고 싶을 때 무엇을 먼저 해야 할까?  
물? 라면? 아니면 동생 소환?  

나는 먼저 라면을 끓일 수 있는 도구들이 필요하다고 생각한다.  
즉, 라면을 끓일 수 있는 불과 라면을 담을 수 있는 냄비가 필요하다.  

그럼, Java 개발하기 전 제일 먼저 해야 하는 것은 무엇일까?  
바로 본인의 컴퓨터에 Java를 개발할 수 있는 툴(JDK)을 설치 하는 것이다.  

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
가입이 완료되었다면 이제 JDK 11을 아래 사이트에서 다운로드 하자.  
[https://www.oracle.com/java/technologies/javase-jdk11-downloads.html#license-lightbox](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html#license-lightbox)  

JDK 버전이 다양하게 있지만 버전 'JDK 11'을 받는 이유는 'JDK 11'이 LTS 버전이기 때문에 설치 한다.  
LTS란 'Long Term Support'의 약자로 장기지원버전이기 때문이다.  
간략히 말하면 안정된 버전이다.  

다운로드 위치는 고급 개발자에 따라 달라질 수 있으나, 통상 기본적으로 C드라이브의 Program Files의 Java 디렉토리를 만들어 저장하도록 하자

    C:\Program Files\Java

