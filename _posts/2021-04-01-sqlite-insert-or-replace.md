---
layout: single
title: '[SQLite] INSERT OR REPLACE'

categories:
  - SQLite

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

데이터베이스를 사용할 때 테이블에 데이터가 이미 있으면 UPDATE, 없으면 INSERT를 해야 하는 경우가 있다.  
Oracle과 SQLServer는 MERGE INTO 구문을 지원하기에 이를 할수 있는데 SQLite에도 비슷한 기능을 하는 문법이 존재한다.

### 문법

```sql
INSERT OR REPLACE INTO[TABLE]
(COLUMN1, COLUMN2, ...)
VALUES
(VALUES1, VALUES2, ...)
```

문법은 기존 INSERT INTO 에 OR REPLACE INTO 가 추가된것 뿐인 형태로 사용할 수도 있고

```sql
REPLACE INTO[TABLE]
(COLUMN1, COLUMN2, ...)
VALUES
(VALUES1, VALUES2, ...)
```

이와 같이 축약하여 REPLACE INTO 만 사용할 수도 있다.

### 사용 예시

```sql
CREATE TABLE employee (
	id integer PRIMARY KEY,
	name text NOT NULL,
	salary numeric
);
```

우선 테이블을 위와 같이 만들어 낸다.

```sql
INSERT INTO employee (name, salary)
VALUES ('Kim', 200),
       ('Seo', 220),
       ('Jeong', 210);
```

위와 같이 데이터를 집어넣는다.

| id  | name  | salary |
| --- | ----- | ------ |
| 1   | Kim   | 200    |
| 2   | Seo   | 220    |
| 3   | Jeong | 210    |

조회시 위와 같이 데이터가 들어있는 것을 확인 할 수 있다.

```sql
REPLACE INTO employee (id, name, salary)
VALUES(4, 'Kang', 200);
```

새로운 데이터를 REPLACE 를 사용하여 집어넣는다.

| id  | name  | salary |
| --- | ----- | ------ |
| 1   | Kim   | 200    |
| 2   | Seo   | 220    |
| 3   | Jeong | 210    |
| 4   | Kang  | 200    |

**id : 4**인 Row는 존재하지 않았기에 위와 같이 새로운 Row가 추가되어있다.

```sql
REPLACE INTO employee (id, name, salary)
VALUES(4, 'Cha', 300);
```

REPLACE 로 같은 내용에서 salary만 바꾸어 집어넣는다.

| id  | name  | salary |
| --- | ----- | ------ |
| 1   | Kim   | 200    |
| 2   | Seo   | 220    |
| 3   | Jeong | 210    |
| 4   | Cha   | 300    |

**id : 4**인 Row 가 존재하기에 위와 같이 새로운 Row의 추가없이 **id : 4**인 Row의 데이터만 바뀌어 있는 것을 확인 할 수 있다.

### 더 알아보기

REPLACE 는 UNIQUE나 PRIMARY KEY 제약조건이 위배됨을 감지하여 제약조건이 위배된 ROW를 찾아 DELETE를 수행하고
명령을 다시 수행하는 형태로 동작하게 된다.
때문에 INSERT OR UPDATE라고 생각하고 사용하면 맞지 않는다.

```sql
CREATE UNIQUE INDEX idx_employee_name ON positions (name);
```

아까 만든 테이블에서 name COLUMN에 UNIQUE 제약조건을 추가 한다.

```sql
REPLACE INTO employee (id, name, salary)
VALUES(5, 'Cha', 300);
```

REPLACE 를 사용해 **id : 5**인 Row를 추가하려 하면 **name : 'Cha'** 가 제약조건에 위배되어 실행되지 않아야 하지만
REPLACE는 충돌이 발생한 Row를 DELETE 시키고 명령을 진행하게된다.

| id  | name  | salary |
| --- | ----- | ------ |
| 1   | Kim   | 200    |
| 2   | Seo   | 220    |
| 3   | Jeong | 210    |
| 5   | Cha   | 300    |

따라서 위와같이 **id : 4**인 Row가 DELETE 되고 **id : 5**인 Row가 Insert 되게 된다.

```sql
REPLACE INTO employee (id, name, salary)
VALUES(3, 'Cha', 200);
```

제약조건이 위배되는 Row가 여러개여도 모두 DELETE 후 명령을 실행하게 되는데, 위와 같이 id와 name Column의 제약조건이 모두 위배되게 REPLACE를 사용한다면

| id  | name | salary |
| --- | ---- | ------ |
| 1   | Kim  | 200    |
| 2   | Seo  | 220    |
| 3   | Cha  | 200    |

Table에서 제약조건이 위배된 모든 Row를 DELETE 후 명령이 실행되어진 것을 확인 할 수 있다.
