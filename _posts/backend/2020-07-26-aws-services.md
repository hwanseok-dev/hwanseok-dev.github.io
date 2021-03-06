---
title: "[Backend][AWS] AWS 서비스 간단 설명"
excerpt: ""
date: 2020-07-26
categories:
  - AWS
tags:
  - aws
toc : false
toc_label: "Table of contents"
toc_icon: "list"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
classes: wide
---


| 서비스 | 설명 |
|:------|:-----|
|EC2|Elastic Compute Cloud.<br> 독립된 컴퓨터를 임대해주는 서비스.<br>터미널에서 ssh를 통해서 ec2의 IP주소로 접속해서 사용한다.|
|S3|Simple Storage Serivce.<br> bool,int,string과 같은 간단한 데이터 타입을 지원하는 저장공간이다.|
|RDS|Relational Database Service.<br>관계형 데이터베이스를 서비스로서 제공하는 제품이다.<br>MySQL, MariaDB, PostgreSQL, SQL Server, ORACLE 등을 직접 운영하지 않고 AWS에 대행할 수 있다.|
|ELB|Elastic Load Balancing.<br> ELB 인스턴스와 EC2 인스턴스를 연결하면 사용자는 ELB로 요청하고 ELB는 EC2를 번갈아가면서 요청을 전달한다.|
|Identitiy & Access Manageent| aws 계정 접속 시 2단계 인증을 추가할 수 있다.|
|SMS| 이벤트 발생시 사용자에게 Text Message를 보내준다.|
|SES| 이벤트 발생시 사용자에게 Email을 보내준다.|
|CloudFront(CDN)|Content Distribution Network.<br> CDN의 역할을 CDN 포스팅을 참조.|
|Lambda| 서버, 서버 위 프로그램 설치 없이 데이터 입력 받고 출력 할 수 있다.<br> 함수를 작성하고 실행은 AWS 내부 서버가 수행한다. 함수를 작성하고 함수가 언제 실행될지만 정하면 된다.<br>클라이언트 - API 게이트웨이 - lambda - DB의 구조를 가지기 때문에 API Gateway를 설정하고 클라이언트가 이 주소로 접속할 수 있도록 해야한다. |
|Mongo DB| Mongo DB를 통해 AWS와 연동이 가능하다.|



