### GameBox 프로젝트 계획서

---

### 1. 프로젝트 이름과 목적
- **프로젝트 이름**: GameBox
- **목적**: JSP 웹 애플리케이션 제작. MVC 디자인 패턴과 Frontcontroller를 사용하는 방식으로 동적 관리 기능 구현. Steam(스팀) 같은 게임 판매 및 커뮤니티 기능을 갖춘 웹사이트.
- **주요 사용자 대상**: 개인 포트폴리오 프로젝트

---

### 2. 주요 기능 및 핵심 요소

- **구현 주요 기능**:
  - 관리자 페이지와 일반 이용자 페이지 구분
  - 로그인 시 관리자 계정으로 접속하면 관리자 페이지로 이동
  - 로그인 시 일반 이용자 계정으로 접속하면 일반 페이지로 이동
  - 관리자 페이지:
    - 상품 등록, 수정, 조회, 삭제 기능
    - 결제 등록, 수정, 조회, 삭제 기능
    - 리뷰 등록, 수정, 조회, 삭제 기능
    - 댓글 등록, 수정, 조회, 삭제 기능
    - 회원 관리(CRUD)
    - 커뮤니티 게시판 관리(CRUD)
  - 일반 이용자 페이지:
    - 게임 상품 검색, 조회, 선택, 결제
    - 구매한 상품을 라이브러리에 보관
    - 게임 상품 리뷰 작성
    - 커뮤니티 게시판 CRUD
    - 마이페이지에서 구매내역 조회

- **우선순위**:
  1. 회원가입/로그인 기능
  2. 관리자 페이지의 기본 CRUD 기능 구현
  3. 일반 이용자 페이지의 데이터 출력 및 UI 개선

---

### 3. 기술 스택
- **사용 기술**:
  - JDK 17
  - Oracle DB XE
  - JSP, JSTL, EL
  - HTML, CSS, JavaScript
  - ojdbc6.jar, jstl.jar, standard.jar
- **목표**: 간단하고 빠르게 구현 가능한 기술 우선 사용

---

### 4. 구조 및 아키텍처

- **설계 패턴**:
  - MVC 패턴과 Frontcontroller 사용

  ```java
  // FrontController.java
  public class FrontController extends HttpServlet {
      protected void service(HttpServletRequest request, HttpServletResponse response) {
          // 요청 URI 분석, Command 확인, Action 클래스 호출
          // 중앙 집중식 예외 처리 및 View로의 경로 전달
      }
  }
  ```

- **연결 방식**:
  - `context.xml`과 `web.xml` 설정
  - `mapping.properties` 파일을 통해 View 페이지 및 Command 관리

---

### 5. 테스트 계획

- **단위 테스트 및 통합 테스트**:
  - 로그인 버튼 클릭 시 로그인 창으로 이동
  - 로그인 성공 시 관리자 계정은 관리자 페이지, 일반 계정은 메인 페이지로 이동
  - 잘못된 아이디/비밀번호 입력 시 알림 출력 및 재입력 가능

- **테스트 시나리오**:
  - 공백, 불일치 데이터 처리 및 예외 상황 알림
  - 세션 관리 및 비로그인 상태에서 페이지 접근 제한

---

### 6. 로그 및 예외 처리

- **로그 처리**:
  - 개발 단계: `System.out.println()`으로 요청/응답 로그 출력
  - 운영 단계: Log4j/SLF4J로 파일 저장 방식 도입 예정

- **예외 처리**:
  - 중앙 집중식 예외 처리로 모든 요청과 응답 관리
  - `WEB-INF` 경로 호출 오류 발생 시 디버깅 로그 추가

---

### 7. 보안

- **사용자 데이터 보호**:
  - 회원가입/로그인 테스트 완료 후 SHA-256 암호화 도입 고려

- **SQL Injection 방지**:
  - PreparedStatement 사용으로 사용자 입력값 처리

- **XSS 방지**:
  - 사용자 입력값을 HTML 이스케이핑하여 출력

- **관리 페이지 접근 제한**:
  - Role 기반 접근 제어 (ADMIN만 접근 가능)
  - URL 직접 접근 시 차단 설정

---

### 8. 확장성 및 유지보수

- **아키텍처 확장성**:
  - 공통 유틸리티(Service, DAO) 분리
  - 기능별 서브 모듈화

- **유지보수 전략**:
  - 코드 모듈화 및 주석 작성
  - 설정 파일 기반 구조화로 추가 기능 지원

---

### 9. 배포 및 호스팅 계획

- **환경 설정**:
  - Tomcat 9, localhost:8585 포트 사용

- **배포 후 성능 모니터링**:
  - 초기 배포 시 모니터링 계획 없음

- **향후 호스팅**:
  - AWS Free Tier, Heroku 등 무료 클라우드 서비스 활용 고려

---

### 10. 프로젝트 문서화

- **문서화 계획**:
  - API 명세서, ERD 다이어그램 작성
  - 코드 구조 및 주요 기능 설명 포함

- **포트폴리오 자료 준비**:
  - PPT 발표 자료 제작
  - GitHub Repository 공개
  - 프로젝트 시연 동영상(GIF/YouTube 링크) 준비

---

### 11. 데이터베이스 설계

- **사용 DB**: Oracle DB XE
- **주요 테이블**:

  ```sql
  CREATE TABLE USERS (
      USER_ID NUMBER PRIMARY KEY,
      USERNAME VARCHAR2(50) NOT NULL UNIQUE,
      PASSWORD VARCHAR2(100) NOT NULL,
      EMAIL VARCHAR2(100) NOT NULL UNIQUE,
      PROFILE_IMAGE VARCHAR2(200),
      CREATED_AT TIMESTAMP DEFAULT SYSTIMESTAMP,
      ROLE VARCHAR2(10) DEFAULT 'USER' CHECK (ROLE IN ('USER', 'ADMIN'))
  );

  CREATE TABLE GAMES (
      GAME_ID NUMBER PRIMARY KEY,
      TITLE VARCHAR2(100) NOT NULL,
      DESCRIPTION CLOB,
      PRICE NUMBER(10,2) NOT NULL,
      GAME_IMAGE VARCHAR2(200),
      CREATED_AT TIMESTAMP DEFAULT SYSTIMESTAMP
  );

  CREATE TABLE PURCHASES (
      PURCHASE_ID NUMBER PRIMARY KEY,
      USER_ID NUMBER NOT NULL,
      GAME_ID NUMBER NOT NULL,
      PURCHASED_AT TIMESTAMP DEFAULT SYSTIMESTAMP,
      FOREIGN KEY (USER_ID) REFERENCES USERS(USER_ID),
      FOREIGN KEY (GAME_ID) REFERENCES GAMES(GAME_ID)
  );

  CREATE TABLE REVIEWS (
      REVIEW_ID NUMBER PRIMARY KEY,
      USER_ID NUMBER NOT NULL,
      GAME_ID NUMBER NOT NULL,
      RATING NUMBER(2,1) CHECK (RATING >= 0 AND RATING <= 5),
      REVIEW_COMMENT CLOB,
      CREATED_AT TIMESTAMP DEFAULT SYSTIMESTAMP,
      FOREIGN KEY (USER_ID) REFERENCES USERS(USER_ID),
      FOREIGN KEY (GAME_ID) REFERENCES GAMES(GAME_ID)
  );

  CREATE SEQUENCE SEQ_USER_ID START WITH 1 INCREMENT BY 1;
  CREATE SEQUENCE SEQ_GAME_ID START WITH 1 INCREMENT BY 1;
  CREATE SEQUENCE SEQ_PURCHASE_ID START WITH 1 INCREMENT BY 1;
  CREATE SEQUENCE SEQ_REVIEW_ID START WITH 1 INCREMENT BY 1;

  INSERT INTO USERS (USER_ID, USERNAME, PASSWORD, EMAIL, ROLE)
  VALUES (SEQ_USER_ID.NEXTVAL, 'admin', 'admin123', 'admin@example.com', 'ADMIN');
  ```

- **확장 가능성**: 추후 다국어 지원 및 추가 기능 테이블 연동 계획

---

이 문서는 GameBox 프로젝트의 전반적인 계획과 설계를 정리한 문서입니다. 이후 작업 진행에 따라 추가 업데이트 및 세부 사항이 보완될 예정입니다.

