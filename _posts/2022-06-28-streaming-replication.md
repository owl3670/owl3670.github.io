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

# Streaming Replication

## 개요

postgreSQL 의 이중화 방법 중 9.0 이상 버전 부터 지원되는 streaming replication 이란 기능에 대해 알아보고자 한다.

## WAL

WAL (Write-Ahead Logging) 은 데이터 무결성을 보장하는 표준 방법으로 핵심 개념은 변경에 대한 로깅을 먼저 수행 한 후 데이터 파일을 변경해야 한다는 것이다.

이러한 절차를 지킨다면 충돌이 발생 시 로그를 사용하여 데이터베이스를 복구 할 수 있으며, ACID의 특성 중 원자성(Atomicity)과 지속성(Durability)을 제공 받을 수 있다.

## Replication 방식

WAL 레코드가 생성되는 즉시 Standby 서버로 레코드를 전송한다 즉 WAL 파일에 데이터가 모두 채워질때까지 대기하고 전송하지 않고 그때그때 레코드가 생성 될때마다 전송한다.

비동기 전송이 기본 방식이며, Standby 서버에 변경이 반영되기 까지 약간의 딜레이는 존재하나 파일 전송 방식에 비해 매우 적은 시간을 소모한다.

## 특징 요약

### **로그 전달**

- Primary 에서 생성된 WAL XLOG 레코드를 주기적으로 standby 서버에 전달함

### **다중 대기 서버**

- Standby 서버를 여러개 구성 할 수 있으며, 로그는 모든 연결된 Standby Server 에 배송됨

### **지속 복구**

- 발송된 로그를 pg_standby 의 사용 없이 계속하여 재생함
- standby 서버에서 주기적으로 복구에 필요없는 오래된 로그 파일을 삭제하여 공간을 절약함

## Test

### **Test 환경**

- OS : Ubuntu 20.04.4 LTS
- PostgreSQL 은 도커 이미지 사용
    - postgres:latest (14.4 버전)
    - Docker image OS : Debian GNU/Linux 11

### **Primary Database Setting**

Primary 에서 Database (streaming_replication), Table (user) 를 만들어 둔 후 Test 데이터를 Insert 해둔 채로 시작

![image](/assets/images/streaming_replication/primary_table1.PNG)

Password 암호화 방식 설정

```sql
SET password_encryption = 'scram-sha-256';
```

- scram-sha-256 은 postgreSQL 10 버전 이후 부터 사용 가능함
- 이전 버전에서는 md5 를 사용하면 됨

Replication 을 위한 User 생성

```sql
CREATE ROLE repuser WITH REPLICATION PASSWORD '<PASSWORD>' LOGIN;
```

postgresql.conf 수정

```
listen_addresses = '*'
wal_level = replica
max_wal_senders = 10
max_replication_slots = 10
synchronous_commit = off
```

- Test 환경 상 파일 경로 : /var/lib/postgresql/data/postgresql.conf
- 파일안에 옵션이 주석처리되어 있으므로 위의 옵션 항목을 찾아 주석 해제 후 사용

Replication slot 생성

```sql
SELECT * FROM pg_create_physical_replication_slot('<SLOT_NAME>');
```

- Replication 이 늘어 나면 slot 생성도 추가적으로 하여야 함
- 후에 Stanby Server 에서 postgresql.conf 파일을 수정할 때 primary_slot_name 에 위에서 생성한 slot 이름을 할당하여야 함

pg_hba.conf 수정

```
host replication repuser <STANDBY_IP>/32 scram-sha-256
```

- Test 환경 상 파일 경로 : /var/lib/postgresql/data/pg_hba.conf

primary 재시작

### **Stanby Database Setting**

pg_basebackup 수행

```bash
pg_basebackup -h <PRIMARY_IP> -D <STANDBY_DATA_DIRECTORY> -U repuser -vP -W
```

- Standby Server 의 Data Directory 는 비워져 있어야 함
- Test 환경 상 Data Directory : /var/lib/postgresql/data
- Docker 환경으로 가동 시 docker run 커맨드와 위의 커맨드를 조합해서 사용 (Test 환경 상 도커 사용)e.g.) docker run -it --rm postgres:latest pg_basebackup -h localhost -D /var/lib/postgresql/data -U repuser -vP -W

standby.signal 파일 생성

```bash
touch <STANDBY_DATA_DIRECTORY>/standby.signal
```

- Test 환경 상 Data Directory : /var/lib/postgresql/data

postgresql.conf 수정

```
primary_conninfo = 'host=<PRIMARY_IP> port=5432 user=repuser password=<PASSWORD> application_name=<APPLIACTION_NAME>'
primary_slot_name = '<SLOT_NAME>'
hot_standby = on
wal_level = replica
max_wal_senders = 10
max_replication_slots = 10
synchronous_commit = off
```

- Test 환경 상 파일 경로 : /var/lib/postgresql/data/postgresql.conf
- 파일안에 옵션이 주석처리되어 있으므로 위의 옵션 항목을 찾아 주석 해제 후 사용
- primary_slot_name 은 Primary 에서 생성한 replication slot 의 이름을 주어야 함

stanby 시작

Stanby 에서 Primary 와 같은 데이터를 확인

![image](/assets/images/streaming_replication/standby_table1.PNG)

### **Streaming Replication 상태 확인**

Database Log 를 통해 streaming 이 시작되었는지 확인 가능

```
LOG:  database system was shut down in recovery at 2022-06-28 00:58:07 UTC
LOG:  entering standby mode`
LOG:  redo starts at 0/2000028`
LOG:  consistent recovery state reached at 0/3000000`
LOG:  database system is` `ready to accept read-only connections`
LOG:  started streaming WAL from primary at 0/3000000 on timeline 1`
```

Primary 에서 쿼리를 통해 Replication Stat을 확인 가능

```sql
SELECT * FROM pg_stat_replication;
```

![image](/assets/images/streaming_replication/pg_stat_replication.PNG)

### **Streaming Replication Test**

Primary 에서 Data Insert

![image](/assets/images/streaming_replication/primary_table2.PNG)

Standby 에서 Data 확인

![image](/assets/images/streaming_replication/standby_table2.PNG)

## Reference

- [https://wiki.postgresql.org/wiki/Streaming_Replication](https://wiki.postgresql.org/wiki/Streaming_Replication)
