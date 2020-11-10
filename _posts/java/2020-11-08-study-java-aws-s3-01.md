---
layout: post
title: "Java AWS S3 SDK V2 연동하여 파일 관리하기"
subheading: Java AWS S3 SDK V2
author: Jeffrey
categories: Java
banner: "/assets/images/banners/java.jpeg"
tags: Java
---

## Java AWS S3 SDK V2 연동하기 

AWS S3 서비스를 이용하여 파일 업로드 및 다운로드 구현하기  

## 목차
    1. 로컬 개발을 위한 localstack을 이용하여 S3 서비스 docker-compose up 하기
    2. AWS Cli 설치 및 로컬 설정하기
    3. AWS 버킷 관련 예제
    4. AWS 오브젝트 관련 예제

### 1. 로컬 개발을 위한 localstack을 이용하여 S3 서비스 docker-compose up 하기

Localstack 이란?  
[LocalStack](https://github.com/localstack/localstack)
```
LocalStack - A fully functional local AWS cloud stack

LocalStack provides an easy-to-use test/mocking framework for developing Cloud applications.
Currently, the focus is primarily on supporting the AWS cloud stack.
```

스프링 부트로 gradle 기본 웹 프로젝트를 생성한다.

웹프로젝트 루트에서 'docker-compose.yml' 을 생성하여 아래와 같이 설정한다.  

```
version: '3.3'

services:
  localstack:
    image: localstack/localstack:0.11.3
    privileged: true
    ports:
      - 8080:8080
      #- 4567:4567   # apigateway
      #- 4568:4568  # kinesis
      #- 4569:4569   # dynamodb
      #- 4570:4570  # dynamodbstreams
      #- 4571:4571  # elasticache
      - 4572:4572   # s3
      #- 4573:4573  # firehose
      #- 4574:4574   # lambda
      #- 4597:4597   # ec2
    environment:
      - DATA_DIR=/tmp/localstack/data
      - DEBUG=1
      - DEFAULT_REGION=ap-southeast-2
      - DOCKER_HOST=unix:///var/run/docker.sock
      - LAMBDA_EXECUTOR=docker-reuse
      - PORT_WEB_UI=8080
      - SERVICES=s3
      - HOSTNAME=localstack
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - localstack:/tmp/localstack/data

volumes:
  localstack:
```
 
설정 후 docker 를 실행한다.    
```
~/projectRoot] docker-compose up
```

그럼 docker 가 localstack 이미지를 다운받고 설정 후 기동된다.  


### 2. AWS Cli 설치 및 로컬 설정하기

터미널에서 AWS Cli 을 설치 한다.  
```
~/projectRoot] brew install awscli
```

설치가 완료 된 후 인증 관련 설정한다.      
```
~/projectRoot] aws configure
~/projectRoot] AWS Access Key ID [****************T6FG]: test
~/projectRoot] AWS Secret Access Key [****************ysGP]: test
~/projectRoot] AWS Secret Access Key [****************ysGP]: test
~/projectRoot] Default region name [ap-northeast-2]: ap-southeast-2
~/projectRoot] Default output format [None]: json
```

설정 완료 후 localstack 의 s3 엔드포인트 및 버킷을 설정 및 생성한다.     
```
~/projectRoot] aws --endpoint-url="http://localhost:4572" s3 mb s3://s3-bucket
```

정상설정이 되었는지 localstack 대시보드로 확인한다.  
[localstack-dashboard](http://localhost:8080/#!/infra)
