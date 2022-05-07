# NoSQL

Not Only SQL  

SQL: mysql, postgresql, SQLite 등이 있다.  
NoSQL이면 그거 빼고 다이다.

기존 관계형 DBMS 특성 뿐만 아니라 추가적인 기능들이 있다.  
기존이 보장하는 ACID(Atomic, Consistency, Integerity, Durability) 특성을 제공하지 않지만 뛰어난 확장성이나 성능을 갖는 비관계형 분산 데이터베이스가 등장했다.  
NoSQL이 등장했어도 관계형 데이터베이스가 많이 쓰였고 언어의 평니성 때문에 NoSQL은 많이 활용되지 못했다.

비정형데이터를 저장 및 처리할 수 있는 구조를 가진 데이터베이스가 점점 필요해지면서 NoSQL이 각광을 받았다.  
단순 검색 및 추가 작업에 있어서 최적화된 키 값 저장 기법을 사용하여 뛰어난 성능을 타나낸다.

- 관계형 모델을 사용하지 않으며 테이블간의 조인 기능 없음
- 직접 프로그래밍을 하는 등의 비SQL 인터페이스를 통한 데이터 액세스
- 대부분 여러 대의 데이터베이스 서버를 묶어서(클러스터링) 하나의 데이터베이스를 구성
- 관계형 데이터베이스에서는 지원하는 Data처리 완결성(Transaction ACID 지원) 미보장
- 데이터의 스키마와 속성들을 다양하게 수용 및 동적 정의 (Schema-less)
- 데이터베이스의 중단 없는 서비스와 자동 복구 기능지원
- 다수가 Open Source로 제공
- 확장성, 가용성, 높은 성능

현재 시장에서 가장 많이 인기가 있는 제품들은 MongoDB(Document), HBase(Wide Columnar Store), Cassandra(Wide Columnar Store) 등이 있다.

- Document DB
json document로 저장한다.
row, column 형태가 아니라 원하는 구조로 저장할 수 있다.

- key value DB
Cassandra, Dynamo DB: 데이터 처리가 매우 빠르다.
sql은 데이터 구조만 고민하지만 여기선 저장 전에 어떻게 사용할지 고민해야한다.

- graph DB
column이나 document 필요 없이, 노드 사이의 관계가 필요하다.
페이스북이 graph DB를 사용했다.
neo4j 가 예시의 DB이다.

대부분은 SQL로 가능하고 NoSQL은 특수한 경우에 사용하는 경우가 많다.


# SQL

Structured Queried Language

Relational Database   
row and column   

ORM: 파이썬에서 코드를 쓰면 SQL로 변환해준다. nodejs도 마찬가지고.   
