# SQL Performance Turning이란?
## Performance turning?
시스템 성능을 향상시키는 것.
일반적으로 컴퓨터 시스템에서 이러한 활동에 대한 동기를 성능 문제라고 하며, 실제로 발생하거나 예상할 수 있다.
대부분의 시스템은 증가된 부하에 어느 정도 반응하여 성능이 저하된다.

## Database turning?
데이터베이스 응용프로그램을 보다 빨리 실행할 수 있도록 하는 활동.
"보다 빨리"는 일반적으로 처리량이 더 높음을 의미하지만, 시간이 중요한 애플리케이션의 경우 응답 시간이 더 짧다는 걸 의미함.

## SQL Turning?
응용 프로그램이 발급할 SQL 문을 최대한 빨리 실행할 수 있도록 보장하는 프로세스.

## 요청 Data 응답이 느린 이유?
- 잘못된 애플리케이션 구성
  * 잘못된 설정, 응용 프로그램 업데이트 안 됨
- 부족한 물리적 하드웨어 리소스
  * 메모리, 디스크, CPU, 네트워크
- 쿼리 옵티마이저가 최상의 계획을 선택하지 않음
  * 옵티마이저를 강제하기 위한 힌트가 필요할 수 있음
- 잘못 작성된 쿼리
  * 커서를 사용하여 쿼리를 더 효율적으로 쓸 수 있음
- 적절한 인덱싱이 없음
  * 테이블의 잘못된 인덱싱, 사용되지 않는 인덱싱, 너무 많은 인덱싱
  
## 성능 조정 및 모니터링에 사용할 수 있는 툴과 어플리케이션?
- Performance monitor
- Activity monitor
- SQL Profiler
- Extended Events
- DMV (Database management views)
- DBCCs (Database console commands)
- Stored procedures (sp_who, sp_monitor)
- SQL Standard Report
- Database tuning advisor
- Execution plans
- Data collection
- Query store
- Other monitoring tools

# Auto-growth
## Auto-growth란?
- 저장 공간이 부족할 때 SQL 서버 엔진이 데이터베이스 파일의 크기를 확장하는 프로세스. 
- 각 데이터베이스에는 모델 데이터베이스의 기본 설정으로 설정된 자체 자동 증가 설정이 있음.

## 각 데이터베이스마다 Auto-growth를 설정하는 이유?
- 자동 증가 이벤트가 수행될 때마다 SQL Server가 데이터베이스 처리를 지연시킴. 이로 인해 성능이 저하될 수 있음.

## Auto-growth 목적과 설정하지 않았을 경우 발생하는 문제
- 자동 증가 이벤트가 발생할 때마다 서버는 이 공간을 확장하고 디스크에 공간을 배치해야 함. 그러나 데이터베이스에 할당된 공간이 디스크의 서로 인접하지 않을 수 있기 때문에 데이터베이스 파일을 조각화하여 성능이 저하됨.
- 새 데이터베이스를 정의할 때 자동 증가를 위한 초기 설정이 기본값으로 설정됨. 이러한 기본값은 모델 데이터베이스 파일의 자동 증가 설정을 사용하여 설정됨.

## 자동 증가 이벤트가 너무 많이 발생하지 않게 하려면?
- 데이터베이스 크기를 미리 조정하고 자동 증가 크기를 적절하게 조정한다.

# Index
## Index란?
- 인덱스의 주요 목적은 데이터 검색 속도를 높이는 것.
- 방문/검색해야 하는 데이터베이스의 데이터 페이지 수(페이지 수가 적을수록 좋음)를 줄임으로써(크기 8kb) 이러한 작업을 수행.
- 전화번호부나 텍스트북처럼 전체를 스캔하지 않고 색인을 사용하여 데이터를 빠르게 검색할 수 있도록 도와주는 인덱스를 가지고 있음.
- 인덱스에는 주로 클러스터된 인덱스와 비클러스터된 인덱스의 두 가지 유형이 있음.

## Clusted index
- SQL Server에서 클러스터된 인덱스는 테이블에 있는 데이터의 실제 순서를 결정.
- 테이블당 클러스터된 인덱스는 하나만 있을 수 있음.
- 인덱스는 테이블로부터 선택한 데이터 열의 복사본으로, 낮은 수준의 디스크 블록 주소 또는 복사된 데이터의 전체 행에 대한 직접 링크를 포함하여 매우 효율적으로 검색함.
- 일부 데이터베이스는 개발자가 함수 또는 식에 색인을 작성할 수 있도록 하여 색인화 기능을 확장.

## Non-clusted index
- 인덱스의 논리적 순서가 디스크에 있는 행의 실제 저장 순서와 일치하지 않는 특수한 유형의 인덱스.
- 비클러스터된 인덱스의 리프 노드가 데이터 페이지로 구성되지 않음. (대신, 리프 노드는 인덱스 행을 포함)
- 인덱스는 인덱스 작성 문을 사용하여 작성된 데이터베이스의 고유한 구조.
- 자체 디스크 공간이 필요하며 인덱스 테이블 데이터의 복사본을 보관함.
- 즉, 인덱스는 순수 이중화로, 인덱스를 생성해도 테이블 데이터가 변경되지 않고 테이블을 참조하는 새 데이터 구조만 생성됨.
- 데이터베이스 인덱스는 책 끝에 있는 인덱스와 매우 유사함.
- 데이터베이스 인덱스는 자체 공간을 차지하고 중복성이 매우 높으며, 다른 장소에 저장된 실제 정보를 나타냄.

## Clusted index 생성시 고려할 점
- 클러스터된 인덱스는 데이터를 하나의 클러스터된 인덱스처럼 키 값에 정렬하고 저장한다.
- 일반적으로 모든 테이블에는 열에 정의된 군집화된 인덱스가 있어야 한다.
- 자주 사용하는 쿼리와 높은 수준의 고유성에 클러스터된 인덱스를 만든다.
- 범위 쿼리에 클러스터된 인덱스 생성 기능을 사용할 수 있다.
- UNIQUE 속성을 사용하여 클러스터된 인덱스 생성.
- BETWEEN, >, >=, <, <=를 사용하여 연산자에 클러스터형 인덱스 생성.
- 큰 결과 세트를 반환.
- JOIN 절, ORDER BY 절 또는 GROUP BY 절 사용
- 고유하거나 고유한 여러 값을 포함하나?
- 순차적으로 액세스 가능하나?
- 테이블에서 검색된 데이터를 정렬하는 데 자주 사용

## Non-clustered index 가이드
- 비클러스터형 인덱스에는 테이블 데이터의 저장 위치를 가리키는 인덱스 키 값과 행 로케이터가 포함됨.
- 테이블 또는 인덱싱된 뷰에 여러 개의 비클러스터형 인덱스가 있을 수 있음.
- 비클러스터형 인덱스는 자주 사용되는 쿼리에 맞게 설계돼야 함.
- 인덱스는 테이블의 정확한 위치를 설명하는 항목이 포함되어 있기 때문에 클러스터되지 않은 인덱스는 정확한 일치 쿼리에 가장 적합.
- 업데이트 요구 사항이 낮은 테이블 - 많이 업데이트된 테이블을 포함하는 데이터베이스는 과도한 인덱싱을 피해야 함.
- 읽기 전용 데이터는 많은 비클러스터된 인덱스의 이점을 얻을 수 있음.
- 테이블의 데이터가 변경될 때 모든 인덱스를 적절히 조정해야 하므로 OLTP 응용 프로그램과 테이블의 수많은 인덱스가 INSERT, UPDATE, DELETE, MERGE 문의 성능에 영향을 미침.
- JOIN, GROUP BY 절 사용
- JOIN, GROUP 작업과 관련된 열에 다중 비클러스터 인덱스 생성.
- WHERE 절에 자주 사용되는 열에 비클러스터 인덱스 생성.
- WHERE 절에 둘 이상의 술어가 있는 경우 해당 열을 포함하여 비클러스터 인덱스 생성. (Covered index)
  * **※ Covered index 생성시 첫번째 열 기준으로 정렬 됨.**
- 쿼리 최적화 도구가 인덱스 내의 모든 열 값을 찾을 수 있는 경우 테이블 또는 클러스터된 인덱스 데이터에 액세스하지 않아 디스크 I/O 작업이 줄어듬.
- 성과 이름의 조합과 같은 고유한 값에 비클러스터 인덱스 생성

## Filter Non-clustered index
- 비클러스터형 인덱스를 작성할 때 인덱스에 필요한 추가 공간을 고려해야 함.
- 솔루션 중 하나는 SQL Server 2008에 도입된 필터링된 인덱스를 사용하는 것.
- 비클러스터형 인덱스 생성할 때 WHERE 절을 넣기만 하면 됨.
- 선택한 레코드만 인덱싱되어 디스크 공간 활용률이 줄어듬.
- 필터링된 인덱스를 사용시 이점
  * 스토리지 비용 절감: 클러스터되지 않은 인덱스에 필요한 Disk 감소
  * 최적화된 쿼리 성능: 데이터가 적다는 것은 최적화 프로그램이 많은 통계를 사용할 필요가 없다는 것을 의미
  * 인덱스 유지 관리 비용 절감: 인덱스 재구축 감소
> 생성 예시
```
--CREATE A FILTER INDEX ON CITY

USE [AdventureWorks2016CTP3]
GO

CREATE NONCLUSTERED INDEX [NonClusteredIndex-FILTER_CITY] 
ON [Person].[AddressB]
([City] ASC) WHERE CITY = 'Seattle'   --<< THE WAY TO CREATE A FILTER NON CLUSTERDE INDEX IS TO ADD THE WHERE CLAUSE
```

# B-Tree
- 인덱스 생성시 인덱스느 인덱스 노드라고 하는 페이지 집합에 분산됨. 이 구조를 B-트리 구조라 부름.
- 페이지는 세 가지 레벨로 분산됨. (루트 노드, 중간 노드, 리프 노드)
- B-트리 색인은 주요 값에 따라 행을 정렬하고 이 순서 목록을 범위로 나눔. (핵심은 관심 있는 열을 기억하는 것)
- 루트 노드가 광범위한 범위 집합을 정의하고 아래 각 수준이 점진적으로 더 좁은 범위를 정의하는 트리 구조.
- 비클러스터된 인덱스의 경우 리프 노드는 행이 존재하는 클러스터된 테이블 또는 힙을 가리킴.

# Page
- SQL Server에서 데이터 스토리지의 기본 단위. 크기는 8KB.
- 3개의 Section으로 구성
  * Page header : 96 byte
  * Rows of data : 8060 bytes per page
  * Row offset : 36 bytes
  
## Page의 용도와 중요한 이유
- 페이지의 8192바이트 중 약 8060바이트가 데이터 행을 삽입할 수 있음. 한 페이지에 들어갈 수 있는 데이터 행을 설계하면 스토리지 사용률이 훨씬 낮아짐.(스토리지 사용 및 페이지 검색 감소로 성능 향상 가능)
  * ex: 행 길이가 4200바이트인 경우 한 페이지에 하나의 행만 저장됨(그리고 나머지 페이지 3860바이트는 낭비되는 공간임). 행 크기를 4000바이트로 재설계할 경우, 한 페이지에 두 개의 행을 저장할 수 있으므로 낭비되는 공간의 오버헤드를 크게 줄일 수 있음.
- 페이지 크기의 관리는 성능 문제를 일으킬 수 있는 페이지 분할 관리에도 중요한 역할을 함.
- SQL 페이지에는 여러 종류가 있음.

|Page Type|Page Names|Description                  |
|---------|----------|-----------------------------|
|1        |Data Page |데이터가 실제로 데이터베이스 데이터 파일에 저장되는 방법에 대한 세부 정보. 클러스터된 인덱스 리프 정보.|
|2        |Index Page|인덱싱 목적으로 사용. 클러스터되지 않은 인덱스 리프 정보.|
|...|... |...|
|10       |IAM Page  |페이지 정보, 메모리 정보를 제한.|
|...|... |...|

## Page split
한 페이지 당 추가, 갱신 가능한 8060바이트의 데이터 공간이 있음. 데이터를 많이 삽입하면 해당 데이터 페이지에서 새로 삽입한 행을 저장할 공간이 부족할 수 있음. 이 경우 SQL 서버는 현재 데이터 페이지의 일부 데이터를 새 데이터 페이지로 이동시킴. 여기에는 행을 삽입하는 일반적인 I/O가 포함되지만 적용 가능한 인덱스를 업데이트하는 것도 포함됨. 이로 인해 과도한 I/O가 발생. 일정량의 페이지 분할은 정상이고 예상되지만 페이지 분할이 너무 많으면 성능 문제가 발생할 수 있음.

> SQL Server에서 Page 분할 상태 확인하기
```
--viewing the actual data in a page that belong to either data page or an index page using DBCC IND command

-- this flag must be turned on

DBCC traceon(3604)
GO

--the following DBCC command wil give all the index info as we have selected -1 option
--when we execute the command below, we see that two pages have been alloacted; one for the IAM and one for data page
--pagetype 10 = IAM  pagetype 1 = data page  2 = index page

DBCC IND ('pages', cars, -1)
GO 

-- To get a more detailed content of the actual data in the data page we use the following DBCC command:

-- this flag must be turned on

DBCC traceon(3604)
GO

--we can actually see the 3 sections of the data page:  header, row content, and row offset

DBCC page('pages',1,304,1) --<< option 1

DBCC page('pages',1,304,3) --<< option 3
```

# SQL Optimizer
- Sql Optimizer(쿼리 최적화 도구)는 가장 효율적인 실행 계획을 쿼리하고 결정하는 SQL Server 엔진의 일부.
- 액세스한 데이터에 대해 수집된 통계를 기반으로 각 SQL 문을 최적화하여 이러한 작업을 수행. 이후 쿼리 최적화 프로그램은 SQL 문을 실행하는 가장 효율적인 방법을 선택.

# Operator

Types of Operators: https://technet.microsoft.com/en-us/library/ms175913(v=sql.105).aspx
- Table Scan  
- Clustered Index Scan 
- Clustered Index Seek  
- Non-clustered Index Seek  
- Compute Scalar  
- Adding a WHERE Clause
- Filter
- Sort  
- Table Joins :
  * Nested Loops Join   
  * Hash Match   (Join)  
  * Merge Join  
- Key LookUp   
- RID LookUp  
- DML statements:
  * Insert  
  * Delete
  * Update
  
# Execution plan
> Tool tip property

- Estimated operator cost : 운영 비용(총 비용의 %)
- Estimated CPU cost : 작업을 완료하는 데 필요한 CPU의 양에 대한 비용
- Estimated I/O cost : 작업을 완료하는 데 필요한 I/O의 양에 대한 비용
- Estimated row size : 작업에 의해 실행된 각 행의 크기
- Actual and Estimated number of rows : 작업에서 실행된 행 수
- Actual and Estimated execution mode : 행 또는 배치에서 행이 한 번에 하나씩 처리되는지 또는 일괄 처리되는지 여부를 나타냄

# Sort operator
- SORT 연산자는 연산자가 받은 모든 행을 순서대로 정렬.
- SORT 연산자를 볼 때 메모리 및 디스크 비용이 많이 들고 성능 문제가 발생할 수 있으므로 실행 계획에서 해당 연산자를 제거하는 것이 좋음.

> SORT로 인한 성능저하를 피하는 가장 쉬운 방법?

SORT 대상 컬럼을 인덱스로 지정. 인덱스는 열별로 정렬되므로 쿼리에 커버링 인덱스를 만들 경우 쿼리 최적화 프로그램이 이 인덱스를 식별하여 SORT 작업을 방지gka.

# Join operator
**NESTED loop join**

작은 테이블에서 사용 중인 열에 인덱스가 없지만 큰 테이블에는 인덱스가 있는 경우
```
SELECT * FROM  Sales.SalesOrderHeader AS OH
WHERE OH.OrderDate BETWEEN '2011-07-01' AND '2011-07-14'  --<< SMALLER TABLE (144 ROWS) (OUTER TABLE) DOES NOT HAVE AN INDEX ON COLUMN ORDERDATE

SELECT * FROM  Sales.SalesOrderDetail  AS OD              --<< LARGER TABLE (121317 ROWS) (INNER TABLE)

SELECT 
OH.OrderDate, OD.OrderQty
FROM  Sales.SalesOrderHeader AS OH 
INNER JOIN
Sales.SalesOrderDetail AS OD 
ON OH.SalesOrderID = OD.SalesOrderID
WHERE (OH.OrderDate BETWEEN '2011-07-01' AND '2011-07-14')  --<< SMALLER TABLE (144 ROWS) (OUTER TABLE) DOES NOT HAVE AN INDEX ON COLUMN ORDERDATE
```
**HASH join**

테이블이 정렬되지 않았거나 인덱스가 없는 경우. 일반적으로 인덱스가 없는 것을 의미.
```
CREATE TABLE  TABLE1 
(id INT identity ,EVENTS varchar(60))

--INSERT SOME DATA 

declare @i int
set @i=0
while (@i<100)
begin
insert into TABLE1 (EVENTS)
select EVENT from AdventureWorks2016CTP3.dbo.DatabaseLog
set @i=@i+1
end

--VIEW LARGE TABLE

SELECT * FROM TABLE1  --<<17900 ROWS

--CREATE A SECOND TABLE

CREATE TABLE  TABLE2 
(id INT identity ,EVENTS varchar(60))

--INSERT SOME DATA

declare @i int
set @i=0
while (@i<100)
begin
insert into TABLE2 (EVENTS)
select EVENT from AdventureWorks2016CTP3.dbo.DatabaseLog
set @i=@i+1
end

--VIEW LARGE TABLE

SELECT * FROM TABLE2  --<<17900 ROWS


--EXECUTE AND VIEW HASH JOIN AS THERE IS NO INDEX

SELECT TABLE1.id, TABLE1.EVENTS, 
TABLE2.id AS Expr1, 
TABLE2.EVENTS AS Expr2
FROM 
TABLE1 
INNER JOIN
TABLE2 
ON TABLE1.id = TABLE2.id
```
**MERGE Join**

Merge되는 테이블 열이 모두 정렬 되있는 경우. 속도가 빠르고 큰 테이블을 조인할 때 더 좋은 성능을 발휘함.
```
SELECT H.CustomerID, H.SalesOrderID, D.ProductID, D.LineTotal 
FROM Sales.SalesOrderHeader H 
INNER JOIN Sales.SalesOrderDetail D ON H.SalesOrderID = D.SalesOrderID 
WHERE H.CustomerID > 100 
```

# Statistics
- 최적화 도구에게 쿼리 명령 집합이 주어지면 최적화 도구는 해당 쿼리를 실행할 수 있는 가장 좋고 효율적인 방법을 선택함. 가장 효율적인 실행 계획을 반환하기 위해 최적화 도구는 테이블과 열의 통계에 의존하여 결과를 얻음.
- Statistics는 표 또는 인덱싱된 뷰의 하나 이상의 열에 있는 값의 분포에 대한 통계 정보를 포함하는 객체임. DBCC 커맨드로 열람 가능.
```
--DBCC SHOW_STATISTICS ( table_name , STATISTIC NAME ) 

DBCC SHOW_STATISTICS ('[Person].[Person]',[IX_Person_LastName_FirstName_MiddleName]) --<< notice the date when it was last updated (NEEDS TO BE UPDATED)
GO  

--REBUILD INDEXES

DBCC SHOW_STATISTICS ('[Person].[Person]',[IX_Person_LastName_FirstName_MiddleName]) WITH HISTOGRAM;  
GO

--Using DBCC FREEPROCCACHE to clear ALL cache

DBCC FREEPROCCACHE

--Using DBCC FREEPROCCACHE to clear specific execution plans from the cache

DBCC FREEPROCCACHE (0x06000500304A1608D05E0EFEE701000001000000000000000000000000000000000000000000000000000000);
```
> Statistics가 필요한 이유?

- 쿼리 최적화 도구는 이러한 통계를 사용하여 쿼리 결과에 반환되는 행 수 또는 카디널리티를 추정함.
- 쿼리 최적화 도구는 이 카디널리티 추정치를 사용하여 인덱스 스캔 연산자 대신 인덱스 검색 연산자를 선택하여 가장 효율적인 실행 계획을 반환함.

> Statistics 작성 시기

- PK를 사용하여 테이블을 생성할 때 SQL Server에서 통계를 자동으로 생성
- 기존 테이블에 색인을 작성할 때
- 인덱스를 재구성할 때 자동으로 새 통계 생성
- 기본적으로 SQL Server에는 자동 생성 통계가 사용되도록 설정돼있음. (절대로 OFF하지 말 것)

# Fill factor
- Fill factor는 각 리프 수준 페이지에서 데이터로 채울 수 있는 공간의 백분율을 결정하며, 각 페이지의 나머지 공간은 향후 성장을 위한 사용 가능한 공간으로 예약함.
- 예를 들어, Fill factor 값을 80으로 지정하면 리프 수준 페이지의 80%에 데이터가 포함되고 나머지 20%는 비어 있게 되며, 데이터가 기본 테이블에 추가될 때 인덱스 확장이 가능한 공간을 제공함. 기본값(100 또는 0)
- 페이지가 완전히 채워지고 완전히 채워진 페이지에 속하는 테이블에 새 데이터가 삽입되면 "페이지 분할" 이벤트가 발생하여 새 데이터를 수용함. Fill factor를 설정하여 이 채워진 정도를 조절할 수 있고 "페이지 분할" 이벤트 발생 빈도를 조절할 수 있음.

> Fill factor를 주시해야하는 이유

특정 인덱스 또는 테이블에 대한 데이터가 있는 5000페이지가 있고 새로 삽입된 데이터를 수용하기 위해 페이지를 반으로 나누면 SQL 엔진이 동일한 양의 데이터를 얻기 위해 더 많은 페이지를 읽어야 하므로 I/O 사용이 더 많이 발생함. 이렇게 추가된 페이지와 높은 I/O로 인해 성능 문제가 발생할 있음.

> Fill factor의 최적의 값?

- 환경에 따라 다름.
- 읽기 전용 데이터베이스가 있는 경우 Fill factor를 100으로 설정.
- INSERT 또는 UPDATE에 대한 수정이 거의 없는 데이터베이스가 있는 경우 Fill factor를 100으로 설정.
- 시스템에 높은 OLTP 트랜잭션이 발생하는 경우 값을 85~95로 낮추는 것이 좋음.

# Locking
동일한 데이터에 동시에 액세스해야 하는 여러 사용자가 있는 환경의 SQL Server는 데이터베이스에 대한 읽기 및 쓰기의 수행 방법을 제어해야 함. 다중 사용자 환경에서 Locking은 **데이터의 무결성**을 데이터베이스에 유지하는 프로세스.

## ACID 테스트

모든 SQL Server 트렌젝션은 ACID 테스트를 통과하도록 강제 됨.

**Atomic(원자성)**: 두 개 이상의 개별 정보가 포함된 트랜잭션에서 모든 정보가 커밋되거나 커밋되지 않음.

**Consistency(일관성)**: 트랜잭션은 새롭고 유효한 데이터 상태를 생성하거나, 오류가 발생할 경우 트랜잭션이 시작되기 전의 상태로 모든 데이터를 반환함.

**Isolation(고립성)**: 처리 중이지만 아직 커밋되지 않은 트랜잭션은 다른 트랜잭션과 격리된 상태로 남아 있어야 함.

**Durability(지속성)**: 시스템이 데이터를 저장하여 장애가 발생하고 시스템이 재시작된 경우에도 데이터를 올바른 상태로 사용할 수 있도록 함.

## Locking in action
- 행, 테이블, 인덱스, 데이터베이스에 잠금을 적용할 수 있음.
- 트랜잭션 중에 행이 잠금에 의해 고정됨.
- 읽기, 삽입, 업데이트, 삭제 중에 잠금이 발생.

> 예시
```
사용자 1이 tom에서 "TOM"으로 테이블 1을 업데이트하려고 함.
사용자 1이 테이블 1을 업데이트하려고 할 때 SQL 서버는 다른 사용자가 ACID 테스트를 완료할 때까지 수정하지 못하도록 이 트랜잭션을 잠급.
동시에 사용자 2는 행 Tom을 "Tom"으로 업데이트하려고 하지만 사용자 1이 트랜잭션을 완료할 때까지 업데이트할 수 없음.
```

## Lock mode
**Exclusive(X)**

한 트랜잭션에 의해 수정되는 데이터를 잠그기 위해 Exclusive 잠금을 사용하므로 다른 동시 트랜잭션에 의한 수정이 방지됨.

**Shared(S)**

읽기 중인 데이터에 대해 Shared 잠금이 유지됨. Shared 잠금이 보류되는 동안 다른 트랜잭션은 읽을 수 있지만 잠긴 데이터를 수정할 수는 없음.

**Update(U)**

Update 잠금은 Shared 잠금과 Exclusive 잠금의 혼합. Update 잠금 자체로는 기본 데이터를 수정할 수 없음. 수정하기 전에 Exclusive 잠금으로 변환해야 함.

**Intent(I)**

Intent 잠금은 트랜잭션이 데이터를 잠그려는 의도를 다른 트랜잭션에 알리는 수단.

**Schema(Sch)**

- Schema 안정성 잠금(Sche-S): 실행 계획을 생성할 때 사용. 이러한 잠금은 개체 데이터에 대한 액세스를 차단하지 않음.
- Schema 수정 잠금(Sch-M): DDL 문을 실행하는 동안 사용. 구조가 변경 중이므로 개체 데이터에 대한 액세스를 차단.

**Bulk update(BU)**
Bulk update 잠금은 대량 작업에 사용.

> Script to find lock info
```
SELECT resource_type, request_mode, resource_description
FROM sys.dm_tran_locks
WHERE resource_type <> ‘DATABASE’
```

# Blocking
한 SPID(SPID 1)가 특정 리소스에 대한 잠금을 유지하고 두 번째 SPID(SPID 2)가 동일한 리소스에 대해 충돌하는 잠금 유형을 획득하려고 할 때 Blocking이 발생.

> SCRIPT TO FIND OPEN TRANSACTIONS
```
SELECT SP.SPID,[TEXT] as SQLCode FROM SYS.SYSPROCESSES SP
CROSS APPLY SYS.DM_EXEC_SQL_TEXT(SP.[SQL_HANDLE])AS DEST WHERE OPEN_TRAN >= 1
```

# Deadlock
- Deadlock은 둘 이상의 트랜잭션에 리소스가 잠겨 있고 다른 세션이 잠긴 세션을 요청할 때 발생.
- 예로 spid 1은 리소스 1에 (X)가 있고, spid 2는 리소스 2에 (X)가 있으며, 두 가지 모두 서로의 리소스에 대한 잠금을 요청함. 이 원형 체인은 Deadlock을 생성.
- Deadlock은 대기하여 해결되지 않음.
- 이 경우 SQL 서버는 Deadlock을 감지하고 프로세스 중 하나를 Deadlock 공격 대상으로 선언하여 프로세스를 삭제함.
