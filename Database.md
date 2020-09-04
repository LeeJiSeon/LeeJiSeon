# Database

## 1. 데이터베이스 풀

### _1) Connection Pool_
* 클라이언트의 요청에 따라 각 어플리케이션의 스레드에서 데이터베이스에 접근하기 위해서는 Connection이 필요하다.
* Connection pool은 이런 Connection을 여러 개 생성해 두어 저장해 놓은 공간(캐시), 또는 이 공간의 Connection을 필요할 때 꺼내 쓰고 반환하는 기법을 말한다.

![connection](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/db-img/db-connection-02.png "connection pool")

#### [DB접근 단계]
1. 웹 컨테이너가 실행되면서 DB와 연결된 Connection 객체들을 미리 생성하여 pool에 저장한다.
1. DB에 요청 시, pool에서 Connection 객체를 가져와 DB에 접근한다.
1. 처리가 끝나면 다시 pool에 반환한다.

![dbconnect](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/db-img/db-connection-01.jpeg "db connect")

#### [connection이 부족할 경우]
* 모든 요청이 DB에 접근하고 있고 남은 Conncetion이 없다면, 해당 클라이언트는 대기 상태로 전환된다.
* Pool에 Connection이 반환되면 대기 상태에 있는 클라이언트에게 순차적으로 제공된다.

#### [사용하는 이유]
* 매 연결마다 Connection 객체를 생성하고 소멸시키는 비용을 줄일 수 있다.
* 미리 생성된 Connection 객체를 사용하기 때문에, DB 접근 시간이 단축된다.
* DB에 접근하는 Connection의 수를 제한하여, 메모리와 DB에 걸리는 부하를 조정할 수 있다.

## 2. 트랜잭션

### _1) 트랜잭션_
> : 데이터베이스의 상태를 변환시키는 하나의 논리적인 작업 단위를 구성하는 연산들의 집합이다.
* 데이터베이스 응용 프로그램은 트랜잭션들의 집합으로 정의 할 수 있다.
* 하나의 트랜잭션은 Commit 되거나 Rollback 된다.
#### [Commit 연산]
* 한개의 논리적 단위(트랜잭션)에 대한 작업이 성공적으로 끝나 데이터베이스가 다시 일관된 상태에 있을 때, 이 트랜잭션이 행한 갱신 연산이 완료된 것을 트랜잭션 관리자에게 알려주는 연산이다.
#### [Rollback 연산]
* 하나의 트랜잭션 처리가 비정상적으로 종료되어 데이터베이스의 일관성을 깨뜨렸을 때, 이 트랜잭션의 일부가 정상적으로 처리되었더라도 트랜잭션의 원자성을 구현하기 위해 이 트랜잭션이 행한 모든 연산을 취소(Undo)하는 연산이다.
* Rollback 시에는 해당 트랜잭션을 재시작하거나 폐기한다.

### _2) 트랜잭션의 성질_
* 원자성(Atomicity), All or nothing
  * 트랜잭션의 모든 연산들은 정상적으로 수행 완료되거나 아니면 전혀 어떠한 연산도 수행되지 않은 상태를 보장해야 한다.
* 일관성(Consistency)
  * 트랜잭션 완료 후에도 데이터베이스가 일관된 상태로 유지되어야 한다.
* 독립성(Isolation)
  * 하나의 트랜잭션이 실행하는 도중에 변경한 데이터는 이 트랜잭션이 완료될 때까지 다른 트랜잭션이 참조하지 못한다.
* 지속성(Durability)
  * 성공적으로 수행된 트랜잭션은 영원히 반영되어야 한다.
  
### _3) 트랜잭션의 상태_

![transaction](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/transaction-status.png)

* 활동(Active)
  * 트랜잭션이 실행 중에 있는 상태, 연산들이 정상적으로 실행 중인 상태
* 장애(Failed)
  * 트랜잭션이 실행에 오류가 발생하여 중단된 상태
* 철회(Aborted)
  * 트랜잭션이 비정상적으로 종료되어 Rollback 연산을 수행한 상태
* 부분 완료(Partially Committed)
  * 트랜잭션이 마지막 연산까지 실행했지만, Commit 연산이 실행되기 직전의 상태
* 완료(Committed)
  * 트랜잭션이 성공적으로 종료되어 Commit 연산을 실행한 후의 상태

## 3. JDBC

### _1) JDBC (Java Database Connectivity)_
> : DB에 접근할 수 있도록 Java에서 제공하는 API

* JDBC는 관계형 데이터베이스에 사용되는 SQL문을 실행하기 위해 자바로 작성된 클래스와 인터페이스로 구성되어 있다.
* 특정 데이터베이스나 특정 데이터베이스 메커니즘에 구애 받지않는 독립적인 인터페이스를 통해 다양한 데이터베이스에 접근하는 코드를 구현할 수 있도록 제공하는 자바 클래스의 표준 집합이다.
* JDBC 클래스는 자바 패키지 `java.sql`과 `javax.sql`에 포함되어 있다.

![jdbc](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/jdbc1.png "JDBC")

#### [DB접근 순서]
1. JDBC driver 로딩
1. Connection 맺기
1. SQL 실행
1. 자원 반환

![jdbc db](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/jdbc_basic_cycle.png)

## 4. 정규화
> : 이상현상이 발생하는 릴레이션을 분해하여 이상현상을 없애는 과정
* 이상현상이 있는 릴레이션은 이상현상을 일으키는 함수 종속성의 유형에 따라 등급을 구분가능
* 릴레이션은 정규형 개념으로 구분하며, 정규형이 높을수록 이상현상은 줄어듬

![NF](https://t1.daumcdn.net/cfile/tistory/99E780335A2A609019)

### <이상현상 (Anomly)>
* 삭제 이상 : 튜플 삭제 시 같이 저장된 다른 정보까지 연쇄적으로 삭제되는 현상
* 삽입 이상 : 튜플 삽입 시 특정 속성에 해당하는 값이 없어 NULL을 입력해야 하는 현상
* 수정 이상 : 튜플 수정 시 중복된 데이터의 일부만 수정되어 일어나는 데이터 불일치 현상

### <함수 종속성>
* 어떤 속성 A의 값을 알면 다른 속성 B의 값이 유일하게 정해지는 관계를 종속성이라 함
* A->B로 표기하며 A를 B의 결정자라고 함 (A가 B를 결정함)

### _1) 제 1 정규형 (1NF)_
> : 릴레이션의 모든 속성 값이 원자값을 갖는 경우

![1NF](https://t1.daumcdn.net/cfile/tistory/99217D335A2A472147 "1NF")

### _2) 제 2 정규형 (2NF)_
> : 제1 정규형을 만족하고, 기본키가 아닌 속성이 기본키에 완전 함수 종속인 경우
* 완전 함수 종속 : 기본키로 묶인 복합키가 존재할 때 복합키(A,B,C)가 모여서 하나의 다른 값(X)을 결정하고 **복합키의 부분집합이 결정자가 되면 안된다**는 뜻이다
* 부분 함수 종속을 제거한다.

![2NF](https://t1.daumcdn.net/cfile/tistory/998E99335A2A47B114 "2NF")

![2NF2](https://t1.daumcdn.net/cfile/tistory/99E5FD335A2A4ED622 "2NF")

```
ex)
- 복합키 : 학생번호, 강좌이름
- (학생번호, 강좌이름) -> 성적
- 강좌이름 -> 강의실 : 부분 함수 종속
=> 완전 함수 종속을 만족시키기 위해 강좌이름과 강의실을 분리해야 한다.
```

### _3) 제 3 정규형 (3NF)_
> : 제2 정규형을 만족하고 기본키가 아닌 속성이 기본키에 비이행적(Non-Transitive)으로 종속하는 경우 (직접 종속)
* 이행적 종속 :  A->B, B->C가 성립할 때 A->C가 성립되는 함수 종속성
* 이행 함수 종속을 제거한다.

![3NF](https://t1.daumcdn.net/cfile/tistory/9913AA335A2A4F5F18 "3NF")

```
ex)
- 학생번호 -> 강좌이름 / 강좌이름 -> 수강료 => 학생번호 -> 수강료
- 학생번호가 수강료를 이행적으로 결정한다.
- 학생번호,강좌이름 / 강좌이름,수강료 테이블로 분리해야 한다.
```

### _4) BCNF_
> : 함수 종속성 X->Y가 성립할 때 모든 결정자 X가 후보키인 정규형

![BCNF](https://t1.daumcdn.net/cfile/tistory/994937355A31375128 "BCNF")

![BCNF2](https://t1.daumcdn.net/cfile/tistory/99A86B335A2A5A4716 "BCNF")

```
ex)
- (학생번호, 특강이름) -> 교수
- 교수 -> 특강이름 : 교수는 결정자이지만 후보키가 아니다.
- 학생번호,교수 / 특강이름,교수 테이블로 분리해야 한다.
```


--- 
## _* 참고_
1. <https://github.com/WeareSoft/tech-interview/blob/master/contents/db.md#4-database>
1. <https://mangkyu.tistory.com/28?category=761304>
