---
layout: single
title: '[PostgreSQL] Streaming Replication'

categories:
  - PostgreSQL

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

## 개요
postgreSQL 의 이중화 방법 중 9.0 이상 버전 부터 지원되는 streaming replication 이란 기능에 대해 알아보고자 한다.

## WAL
WAL (Write-Ahead Logging) 은 데이터 무결성을 보장하는 표준 방법으로 핵심 개념은 변경에 대한 로깅을 먼저 수행 한 후 데이터 파일을 변경해야 한다는 것이다.   
이러한 절차를 지킨다면 충돌이 발생 시 로그를 사용하여 데이터베이스를 복구 할 수 있으며, ACID의 특성 중 원자성(Atomicity)과 지속성(Durability)을 제공 받을 수 있다.

## Replication 방식
WAL 레코드가 생성되는 즉시 Standby 서버로 레코드를 전송한다 즉 WAL 파일에 데이터가 모두 채워질때까지 대기하고 전송하지 않고 그때그때 레코드가 생성 될때마다 전송한다.   
비동기 전송이 기본 방식이며, Standby 서버에 변경이 반영되기 까지 약간의 딜레이는 존재하나 파일 전송 방식에 비해 매우 적은 시간을 소모한다.

## 특징 요약
### 로그 전달
- Primary 에서 생성된 WAL XLOG 레코드를 주기적으로 standby 서버에 전달함
### 다중 대기 서버
- Standby 서버를 여러개 구성 할 수 있으며, 로그는 모든 연결된 Standby Server 에 배송됨
### 지속 복구
- 발송된 로그를 pg_standby 의 사용 없이 계속하여 재생함
- standby 서버에서 주기적으로 복구에 필요없는 오래된 로그 파일을 삭제하여 공간을 절약함

## Test
### Test 환경
- OS : Ubuntu 20.04.4 LTS
- PostgreSQL 은 도커 이미지 사용
    - postgres:latest (14.4 버전)
    - Docker image OS : Debian GNU/Linux 11

### Primary Database Setting
Primary 에서 Database (streaming_replication), Table (user) 를 만들어 둔 후 Test 데이터를 Insert 해둔 채로 시작
![Image Not Found](/assets/images/streaming_replication/primary_table_1.PNG)
Password 암호화 방식 설정
```SQL
SET password_encryption = 'scram-sha-256';
```
> - scram-sha-256 은 postgreSQL 10 버전 이후 부터 사용 가능함
> - 이전 버전에서는 md5 를 사용하면 됨