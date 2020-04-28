--------------------
# Oracle-sql
--------------------

- [1. SQL 이란?](#SQL-이란?)
    - [1. 과거 DBMS](#과거-DBMS)
    - [2. 참조 모델](#참조-모델)
    - [3. 단점](#단점)
        - [1. 동시성](#동시성)
        - [2. 성능](#성능)
        - [3. 보안](#보안)
    - [4. 단점 해결방안](#단점-해결방안)


# SQL 이란?

SQL(Structured Query Language)

`DBMS` 에게 `질의`하는 명령어
DBMS 란 `Database + Management System` 

## 과거 DBMS

네트워크는 연결되어 있지 않은 상태로 
각각의 Data 를 각각의 `다른 장소에서 보관`하고 있습니다.

사용자는 Person 입니다.
`Person 은 지금 A 의 권한`을 얻었습니다.

~~~
A { a, b }
B { a, c , A }
~~~

Person 가 B 에 접근하려 합니다.
하지만 `B 에 접근하려면 A 의 정보 조회가 필수` 입니다.
그러므로 `B 가 A 의 정보를 가지고` 있습니다.

하지만 문제가 하나 발생합니다. 
Person 이 B 를 조회하려 하지만 `방금 A 의 권한을 얻은 Person 의 A 의 정보`는 
`B 에 존재하지 않아 접근할 수 없습니다.`

만약 Person 이 접근하려면 `A 와 B 의 동기화 작업이 완료`되어야만 합니다.

좀더 효과적으로 데이터의 결함을 없앨 수 있는 방법을 찾자면 Data 를 각자 
중복해서 가지고 있지 않도록 `한 곳으로 모아서 관리`하는 것입니다.

한 곳에서 관리하므로 B 가 가지고있는 A 의 정보는 중복되므로 삭제합니다.
그리고 A and B 둘다 가지고있는 a 정보도 중복으로 가지고 있으므로 한곳의 a 를 삭제 합니다.

~~~
A { a, b }                  
B { a, c , A }      =>      C { a, b, c }
~~~

## 참조 모델

이전에는 A 의 정보를 읽어서 Data 를 확인하면 되었지만 지금은 중복의 없애므로서 
테이블의 Data 가 여러 개로 나누어 져 있고 정보를 조회하려면은 나눠진 정보를 조합(참조)해야만 합니다.

- 참조하는 방법
    - 계층형 Hierarchical DBMS
    - 네트워크 Network DBMS
    - 객체형 Object-Oriented DBMS
    - `관계형 Relational DBMS`

장점이 존재하면 단점도 존재합니다.

## 단점

### 동시성

(Person Min) 와 (Person Jun) 가 동시에 B 에 접근하는 경우가 발생하는 경우 `병목현상이 발생`합니다.

### 성능

이전에는 각각의 장소에서 Data 를 관리하여 `이용자의 접근이 분산`되어 성능상 문제가 되지않았지만
한곳으로 모아진 Data 에서는 이용자의 접근이 `한곳으로 모였으므로 이용자가 기다리게 되는 문제`가 발생합니다.

### 보안

네트워크 상태를 기반으로 관리하다 보니까 여러 가지 보안 문제가 발생할 수도 있습니다.

## 단점 해결방안

Data 를 직접 관리하는 것이 아니라 (동시성, 성능, 보안) 을 책임질 수 있는 `관리자`를 하나 만듭니다.

동시접근이 발생하는 경우 관리자가 순서대로 진행할 수 있도록 관리하고
외부인이 접근하는경우 관리자가 인증을 요구하여 보안성을 높입니다.

결국 사용자는 관리자에게 부탁만 하면 됩니다.
그런 부탁(질의)이 `Query Language` 입니다.

결과값이 단순한 값이 아닌 고차원적인 구조화된 정보를 원합니다.
구조화된 정보가 `Structured` 입니다.

- 관리자에게 질의하는 값으로는 사용자가 어떤 Data 를 사용할 것인지 전달합니다.
    - DDL : create/alter/drop

- 서로간의 Data 의 구조를 정의하고 다음에는 Data 를 입력해달라고 합니다.
    - DML : select/insert/update/delete

- 관리자가 접근하는 사용자가 질의를 할 수 있는지 허가할 것인가 를 결정하는 제어명령어가 있습니다.
    - DCL : grant/revoke

# Oracle 설치

http://oracle.com/technetwork/database/database-technologies/express-edition/downloads/index-083047.html

알집 푼다음 setup.exe 관리자권한으로 실행합니다.

추가적으로 보기편한 도움 프로그램도 다운로드 설치합니다.

SQL Developer

https://www.oracle.com/tools/downloads/sqldev-downloads.html

# Seed PDB를 이용한 Pluggable 데이터베이스 생성

~~~
CREATE PLUGGABLE DATABASE jjunprodb 
ADMIN USER dba1
IDENTIFIED BY password
~~~

가상의 데이터베이스를 만드는것과 같은 명령어
모델이되는 SEED 를 복사해서 XEPDB1 생성합니다.

~~~
SEED 복사 -> XEPDB1, XEPDB2, XEPDB3 .....
~~~

오라클에서 기본 테스트로 제공하는 XEPDB1 하나 존재합니다.
테스트 XEPDB1 접근하는 명령문 예제 아래참조

~~~
SQL> select name from v$pdbs;

NAME
PDB$SEED
XEPDB1
~~~

> SQL Developer 실행

데이터베이스 접속 선택

PDB1 서버를 하나 복사합니다.
SID 대신에 서비스 이름을 선택 후 `XEPDB1` 작성 후 테스트 합니다.

## 원격 접속을 위한 설정 변경

~~~
EXEC DBMS_XDB.SETLISTENERLOCALACCESS(FALSE);
~~~

## Admin Accounts

- SYS
    - SYSDBA 를 가지고 있으므로 데이터 베이스 전체를 관리합니다.
- SYSTEM
    - 일반적으로 데이터 베이스를 관리하는 계정
    - 백업, 스케줄링 등등 큰 단위의 데이터 베이서 권한은 없습니다.

## PDB 서버에 system 계정으로 접속하기

> 보기 -> DBA + -> 생성한 PDB 서버 접속

하단에 DBA 인터페이스 표시

> 저장 영역 -> 테이블스페이스 -> 새로 생성

이름, 디렉토리, 파일크기, 추가적으로 자동확장도 설정가능 <br>
예제로 첫번째 `영구`적인 NEWLEC_TABLESPACE 이름의 테이블 스페이스 생성 <br>
두번째 `임시`의 NEWLECT_LOGSPACE 이름의 테이블 스페이스 생성

이렇게 정보를 저장하는 테이블스페이스와 정보를 임시로 저장하는 테이블스페이스가 생성되었습니다.

다음 사용자 추가

> DBA -> 보안 -> 사용자 -> 새로 만들기

사용자 이름 NEWLECT <br>
비밀번호 root

사용자 설정 후 기본 테이블스페이스, 임시 테이블스페이스 설정 <br/>
사용자가 사용할 수 있는 권한을 지정할 수 있습니다. <br/>
일반 유저는 가질 수 없는 권한을 설정해 줘야 합니다. <br/>
시스템 권한 클릭 후 `모두 권한 부여` 클릭 `SYSRAC, SYSOPER, SYSKM` 해제 후 적용

유저의 권한 생성까지 완료했으니 새롭게 데이터베이스에 접근해 보겠습니다.

사용자의 이름을 NEWLEC 비밀번호는 root 로 접근해보면 정상접근이 되었습니다.