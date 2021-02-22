# JPA 공부
## 구성
- ch02 ~ ch12 + clean.sh : [자바 ORM 표준 JPA 프로그래밍 - 책 예제](https://github.com/holyeye/jpabook)

## 삽질
### H2 데이터베이스 설치
H2 데이터베이스를 설치하고 바로 JDBC URL: `jdbc:h2:tcp://localhost/~/test` 로 접속하려고 하면 여러가지 
에러를 맛볼 수 있다.

```
Database "/User/.../test" not found, either pre-create it or allow remote database creation
(not recommaneded in secure enviroments)
```

```
Database "/User/.../test" not found, and IFEXISTS=true, so we cant auto-create it
```

등...

먼저 근본적인 문제점은 `~/test` 라는 데이터베이스가 없다는 것이다. 물론, 구글링해서 데이터베이스를 미리 
생성해보려 해도 안될 때가 많았다. 아래의 과정으로 설치해서 해결했다.

#### 0. 설치 환경
- macOS Catalina 10.15.4

#### 1. H2 데이터베이스 다운로드
- 링크: <http://www.h2database.com/html/download.html> 
- 설치 버전: Version 1.4.199 (2019-03-13), Last Stable

#### 2. H2 데이터베이스 실행
- 다운로드 받은 zip 파일 압축 풀기
- 터미널 실행

```shell
cd h2/bin
chmod +x h2.sh
./h2.sh
```

#### 3. H2 콘솔 접속
- 위 2번 방법으로 실행하면, 웹 페이지가 열리고 무한 로딩이 시작된다.
- IP 부분만 `localhost` 로 변경하자.
    - URL 뒷 부분 권한을 유지해야 함.
    
#### 4. test 데이터베이스 로컬에 생성하기
- 위 3번 과정을 수행하면, 웹 페이지 화면에 H2 콘솔 로그인 창이 열린다.
- 다음과 같이 설정하여, 연결하자.
    - 저장한 설정: Generic H2 (Embedded)
    - 설정 이름: Generic H2 (Embedded)
    - 드라이버 클래스: org.h2.Driver
    - JDBC URL: jdbc:h2:~/test
    - 사용자명: sa
    - 비밀번호: [입력하지 않음]
- 연결 버튼 클릭
    - 구글링 해보면, 연결 시험 버튼을 누르지 않은 상태에서 해야 한다고도 함.
- 연결이 정상적으로 수행되면, 콘솔은 데이터베이스 정보창으로 넘어간다.
    - _~/_ 경로에 `test.mv.db` 파일이 생성되어야 한다.

#### 5. test 데이터베이스 서버 연결하기
- 다시 H2 콘솔에 접속
- 다음과 같이 설정하여, 연결하자.
    - 저장한 설정: Generic H2 (Server)
    - 설정 이름: Generic H2 (Server)
    - 드라이버 클래스: org.h2.Driver
    - JDBC URL: jdbc:h2:tcp://localhost/~/test
    - 사용자명: sa
    - 비밀번호: [입력하지 않음]
- 연결 버튼 클릭

TCP로 연결하는 이유는 로컬에서나 리모트 등에 동시에 접속해도 에러가 나지 않기 때문이다.

#### 6. Member 테이블 생성하기

```sql
CREATE TABLE MEMBER (
    ID VARCHAR(255) NOT NULL, --아이디(기본 키)
    NAME VARCHAR(255),        --이름
    AGE INTEGER NOT NULL,     --나이
    PRIMARY KEY (ID)
);
```

- 콘솔 화면에 보이는 SQL 문 입력창에 위를 입력한 후, 실행을 클릭한다.
- 실행 후, 좌측 섹션에 정상적으로 MEMBER 테이블이 생성됐는지 확인.

#### 참고자료
-  <https://hothoony.tistory.com/890>