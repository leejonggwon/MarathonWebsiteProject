# 1. 서비스소개 

<p align="center">
  <img src="https://github.com/user-attachments/assets/79156a85-c85a-4486-8291-2b55ccd44b4e" width="60%" />
</p>


### 1-1. 서비스명
- 2026 Hankuk Marathon Web Site
- Spring MVC 아키텍처 패턴을 기반으로 설계된 '마라톤대회 안내 및 기록조회 플랫폼'<br>


### 1-2. 서비스설명
- 본 프로젝트는 Spring MVC 아키텍처 패턴을 기반으로 설계된 '마라톤대회 안내 및 기록조회 플랫폼' 입니다 <br>
- Controller, Service, Mapper(Model) 계층을 분리하여 웹 애플리케이션의 유지보수성과 확장성을 높였습니다 <br>
- FAQ, 코스 정보, 공지사항 등 전반적인 안내 기능과 개인 기록 조회 서비스를 제공하여 사용자와의 소통을 지원합니다 <br>
- AJAX 비동기 통신 기술을 적용하여 화면 새로고침 없는 실시간 데이터 갱신을 구현함으로써 시스템 리소스를 최적화하고 사용자 편의성을 극대화했습니다 <br>
- Bootstrap 프레임워크를 활용하여 직관적이고 일관성 있는 UI를 구축했으며, 이를 통해 사용자 친화적인 웹 환경을 구현했습니다 <br>


### 1-3. 프로젝트기간
- 2025.07 ~ 2025.09 <br>
- renewal - 2026.06 <br>


### 1-4. 시연영상
[https://www.youtube.com/watch?v=HDL472NQWXA](https://youtu.be/vmf_k3DFINk)
<br>
<br>

# 2. 기술스택
- **Language** - Java 1.8 <br>
- **Framework** - Spring Framework 5.0.7.RELEASE <br>
- **Database** - MySQL 5.1 + MyBatis 3.4.6 <br>
- **Web Layer** - JSP, JSTL, Servlet 3.1, jQuery, AJAX, HTML/CSS <br>
- **Logging & Utilities** - SLF4J, Log4j, Lombok <br>
- **Database Connectivity** - HikariCP, Spring JDBC <br>
- **Development Tools** - eGovFrame 4.0.0, Eclipse, Apache Tomcat 9 <br>
<br>


# 3. System Architecture 
Spring MVC 구조를 적용하여 비즈니스 로직과 프레젠테이션 계층을 명확히 분리하고, MyBatis Mapper 인터페이스를 활용해 생산성과 유지보수성을 높인 웹 애플리케이션을 개발했습니다
<p align="center">
  <img src="https://github.com/user-attachments/assets/4ee547ea-b48c-452f-90de-94a6e7d2aded" width="60%" />
  <br>
   [Spring Web MVC Framework]
</p>


# 4. DataBase E-R Diagram
<p align="center">
  <img src="https://github.com/user-attachments/assets/faa1f333-29c0-4f39-b495-8a8e97427c45" width=60% />
  <br>
  [E-R Diagram]
</p>
<br>

# 5. 기능구조도
<p align="center">
  <img src="https://github.com/user-attachments/assets/6f24b3d8-b193-43e0-9720-eaed34f23e1f" width=60% />
  <br>
  [기능구조도]
</p>
<br>


# 6. 페이지별 핵심기능가이드
<br>

## 1.  메인 랜딩 페이지 (Main Landing Page)
JSP 템플릿 엔진과 Bootstrap 3, 그리고 jQuery 동적 이벤트를 활용하여 사용자가 대회의 주요 정보(대회소개, 개요, FAQ, 코스)를 단일 웹 화면에서 리로드 없이 탐색할 수 있도록 설계된 마라톤 대회 메인 포털 시스템입니다 <br>
<br>

### 핵심 기술 및 기능적 특성

- **1) 비동기식 탭(Tab) 전환 레이아웃** <br>
  - Bootstrap 3의 `nav-tabs` 및 `tab-content` 레이아웃를 적용하여 페이지 리로드(Refresh) 과정 없이 동적으로 대회의 카테고리별 정보를 노출합니다 <br>
  - 이미지 리소스 및 텍스트 데이터를 사전에 돔(DOM) 트리에 배치한 후 상태값 변환 구조를 사용해 트래픽 낭비와 렌더링 지연을 최소화했습니다 <br>
<br>

- **2) jQuery 기반 인라인 아코디언 토글 인터랙션 (FAQ)** <br>
  - 자주 묻는 질문(FAQ) 섹션의 공간 활용 효율성을 극대화하기 위해 인라인 아코디언 컴포넌트를 직접 구현했습니다 <br>
  - 사용자가 자주묻는질문 영역(`.accordion-title`)을 클릭하면, jQuery 이벤트 리스너가 바로 다음 노드 요소인 답변 영역(`.accordion-content`)을 추적하여 `.toggle()` 처리를 수행하므로 직관적인 뷰 인터랙션을 보장합니다 <br>
<br>

- **3) JSTL / EL 및 jQuery 연동 조건부 알림 모달 (Modal System)** <br>
  - 회원가입 완료나 예외 케이스 처리 시 Controller 에서 넘겨받는 파라미터(`msgType`, `msg`)의 유무를 서버 사이드 렌더링 단계에서 판별합니다 <br>
  - `msgType`이 "성공메세지"로 판별될 경우, `.attr()` 함수를 동적으로 구동하여 `#myMessage` 모달창을 자동으로 활성화합니다 <br>
<br>

- **4) 외부 저장소 정적 웹 리소스 매핑** <br>
  - 웹 애플리케이션 내부(WAR)에 파일을 저장할 경우 재배포 시 파일이 삭제되는 문제를 방지하기 위해 외부 로컬 디렉토리를 지정했습니다 <br>
  - `servlet-context.xml` 설정을 통해 외부 물리 경로를 가상 웹 경로로 매핑함으로써, 보안을 유지하면서도 클라이언트 화면에 이미지를 정상적으로 렌더링할 수 있도록 지원합니다.<br>
<br>

- **5) 공통 템플릿 모듈화 및 컨텍스트 절대 경로 제어** <br>
  - 레이아웃의 중복을 방지하고 단일 관리체계를 구축하기 위해 상단 글로벌 내비게이션 바 영역을 분리하여 구조화했습니다 <br>
  - 웹 애플리케이션의 컨텍스트 루트 경로 유실을 원천 차단하기 위해 JSTL `<c:set>` 태그로 `contextPath` 변수를 고정 배치하여 스타일시트 및 서버 이미지 소스 리소스의 가상 절대 경로 안정성을 확보했습니다 <br>
<br>


<p align="center">
  <img src="https://github.com/user-attachments/assets/23b33a36-59ca-44bb-aab3-fff44771a2f7" width="80%" />
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/ee349085-3de5-4dc2-9246-d71e1725140e" width="80%" />
  <br>
   [랜딩페이지 - 대회소개]
</p>
<br>
<br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/cdf6cfb2-1066-4daa-ab61-061bdbd30b94" width="80%" />
  <br>
   [랜딩페이지 - 대회개요]
</p>
<br>
<br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/bd748062-0210-42de-b3e6-701a87b52a9d" width="80%" />
  <br>
   [랜딩페이지 - 자주묻는질문]
</p>
<br>
<br>


<p align="center">
  <img src="https://github.com/user-attachments/assets/a68af6cc-7944-407a-9a51-3f451d2dbc76" width="80%" />
  <br>
   [랜딩페이지 - 코스안내]
</p>
<br>
<br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/b29b71dc-84b7-4cd7-95c4-ed3ddb73532e" width="80%" />
  <br>
   [랜딩페이지 - 로그인 모달창]
</p>
<br>


## 2. AJAX 기반 비동기 공지사항 게시판
Bootstrap 3 UI 프레임워크와 jQuery 비동기 통신(AJAX)을 활용하여 구현한 공지사항 게시판입니다 <br>
전체 페이지의 새로고침없이 게시글의 생성, 조회, 수정, 삭제(CRUD)가 실시간으로 처리되며, 이미지 파일 유효성 검증 기능이 포함되어 있습니다 <br>
<br>

### 2-1. 웹 리로드 없는 비동기(AJAX) CRUD 및 RESTful 연동
- **전체 목록 조회** - 웹 페이지 로드 시 및 페이징 버튼 클릭 시 테이블 본문(`#view`)을 JSON 데이터를 기반으로 동적 재생성합니다 <br>
- **실시간 본문 상세 보기** - 게시글 제목 클릭 시 해당 글의 본문 영역(`<tr>`)을 슬라이드 토글 방식으로 확장하여 표시합니다 <br>
- **게시글 등록** - `FormData API`를 이용하여 텍스트 데이터와 대용량 이미지 파일을 동시에 비동기 전송합니다 <br>
- **인라인 수정** - 별도의 이동 없이, 테이블 내부에서 즉시 수정 가능한 (`<input>`, `<textarea>`) 폼으로 동적 전환됩니다 <br>
- **게시글 삭제** - 데이터베이스 레코드 삭제 후, 리스트 가시 영역을 새로고침 없이 즉시 동기화합니다 <br>
<br>
  
### 2-2. 유연한 첨부파일 상태 관리 UI
- **조건부 컴포넌트 토글** - 수정 모드 진입 시, 기존 첨부파일의 유무에 따라 [기존 파일 삭제 버튼] 또는 [신규 파일 업로드 input]이 유기적으로 토글되어 화면에 표시됩니다 <br>
- **물리명 정제 로직** - 고유성 확보를 위해 `UUID`가 결합된 서버 측 파일명에서 문자열 파싱(`substring`, `indexOf`)을 적용하여, 사용자에게는 순수 원본 파일명만 정제하여 노출합니다 <br>
<br>

<p align="center">
    <img src="https://github.com/user-attachments/assets/5781f514-3fe0-45e8-a4ce-8cd5ce585da3" width="80%" />
    <br>
     [게시글수정]
  </p>

### 2-3. 세션 기반 동적 UI 권한 관리
- JSTL `<c:if>` 문을 분기하여 세션 내 유저 객체(`mvo`)가 존재할 때만 글쓰기 버튼(`goForm()`) 및 입력 양식을 활성화합니다 <br>
- 자바스크립트 동적 UI 생성 구문 내에서 현재 로그인한 계정 아이디와 게시글 작성자의 소유권 아이디를 엄격히 비교하여, 일치하는 소유자에게만 [수정화면], [삭제] 버튼을 노출시킵니다 <br>
<br>

   <br>
    <p align="center">
      <img src="https://github.com/user-attachments/assets/ee4869b9-f860-41e0-9f6d-9c49b7c989f0" width="80%" />
    </p>
    <p align="center">
      <img src="https://github.com/user-attachments/assets/8e75b958-b312-4794-81e0-636242f36c4f" width="80%" />
      <br>
       [공지사항 게시판]
    </p>
  

## 3. 비동기 파일 업로드 시스템 (Asynchronous File Upload)
JSP Form 데이터와 이미지 파일을 `FormData`를 통해 비동기(AJAX)로 전송하고, 서버 측에서 `Cos` 라이브러리의 `MultipartRequest` 및 `UUID`를 활용하여 안전하게 파일을 서버 디렉토리에 저장하는 업로드 시스템입니다 <br>
<br>

### 3-1. 시스템 구성 및 흐름 
   **1. 클라이언트 (JSP/HTML)** <br> 
     &nbsp; `enctype="multipart/form-data"` 속성을 가진 `<Form>` 태그 구성 및 파일 선택 <br>
   **2. 비동기 요청 (JavaScript/AJAX)** <br>
     &nbsp; 자바스크립트 `FormData` 객체를 생성하여 멀티파트 데이터를 직렬화 없이 서버로 비동기 전송 <br>
   **3. 서버 파싱 (Controller)** <br>
     &nbsp; `MultipartRequest`를 이용하여 대용량 파일 저장 경로 및 최대 크기를 지정하고, 파일과 텍스트 파라미터를 분리 파싱 <br>
   **4. 고유 파일명 생성 (File Rename)** <br>
     &nbsp; 중복 방지를 위해 `UUID`를 생성하고, 기존 파일명과 결합하여 고유한 파일명으로 서버 디렉토리에 최종 저장 <br>
   **5. 정적 리소스 매핑 (Spring Web MVC)** <br>
     &nbsp; 업로드된 외부 디렉토리 경로를 웹 가상 경로로 매핑하여 클라이언트가 접근할 수 있도록 설정 <br>
<br>

### 3-2. 주요 구현 특징
- **FormData 기반의 Asynchronous 전송** <br>
  - 전체 페이지 새로고침없이 데이터를 전송하기 위해, 자바스크립트 내장 객체인 `FormData`를 활용하여 폼 데이터를 캡슐화합니다 <br>
  - AJAX 요청 시 `processData: false` 및 `contentType: false` 설정을 필수적으로 적용하여, 브라우저가 데이터를 쿼리 스트링으로 변환하거나 잘못된 Content-Type 헤더를 설정하는 것을 방지합니다 <br>

- **UUID 기반의 파일명 중복 방지 및 보안 강화** <br>
  - 동일한 파일명을 가진 사용자가 업로드할 경우 발생할 수 있는 파일 덮어쓰기 문제를 해결하기 위해 `UUID.randomUUID()`를 활용합니다 <br>
  - `DefaultFileRenamePolicy`로 1차 처리된 파일에 고유 서명(UUID)을 결합하여, 서버 물리 경로에 저장함으로써 데이터 무결성을 보장합니다 <br>

- **서버 디렉토리 자동 생성 기능** <br>
  - 서버 구동 중 해당 업로드 경로가 존재하지 않을 경우를 대비하여, `File.exists()` 검증을 거쳐 `mkdirs()` 메서드로 필요한 상위 디렉토리까지 안전하게 자동 생성하도록 예외 처리를 강화했습니다 <br>

- **외부 저장소 정적 웹 리소스 매핑** <br>
  - 웹 애플리케이션 내부(WAR)에 파일을 저장할 경우 재배포 시 파일이 삭제되는 문제를 방지하기 위해 외부 로컬 디렉토리를 지정했습니다 <br>
  - `servlet-context.xml` 설정을 통해 외부 물리 경로를 가상 웹 경로로 매핑함으로써, 보안을 유지하면서도 클라이언트 화면에 이미지를 정상적으로 렌더링할 수 있도록 지원합니다.<br>

- **프론트엔드 파일 유효성 검사 (MIME Type Validation)** <br>
  - `File.type` API를 활용하여 선택된 파일의 MIME 타입을 검사하고, 이미지 계열(image/jpeg, image/png 등)이 아닐 경우 업로드를 즉시 차단합니다 <br>
  - 유효하지 않은 파일일 경우 alert 안내를 띄운 후, 파일 입력 창 값을 비워 초기화하고 `return false`로 폼 제출(Submit) 프로세스를 중단시킵니다 <br>
  - 이 처리를 통해 서버의 무리한 파싱 작업을 줄이고 불필요한 네트워크 트래픽을 방지하여 애플리케이션의 안정성을 높였습니다 <br>
<br>
<p align="center">
  <img src="https://github.com/user-attachments/assets/5f0a5cfd-a656-4da5-8635-2e17d3612e14" width="80%" />
  <br>
   [파일업로드 기능]
</p>
<br>


## 4. 비동기 RESTful 페이징 시스템 (Asynchronous Pagination System)
사용자가 요청한 페이지 번호에 맞춰 필요한 범위의 데이터만 DB에서 조회(Offset 기반 페이징)하고, 하단 페이지 블록을 동적으로 계산하여 화면 리로드 없이 페이징을 수행하는 고성능 비동기 시스템입니다 <br>
<br>

### 4-1. 시스템 구성 및 흐름
   **1. 페이지 요청 (Client)** <br>
   &nbsp; 사용자가 하단 페이지 번호를 클릭하면 자바스크립트가 `page` 변수 값을 갱신하고 AJAX 요청을 전송합니다 <br>
   **2. 파라미터 수집 (Controller)** <br>
   &nbsp; Spring MVC가 요청 파라미터를 `Criteria` 객체로 바인딩하여 현재 페이지 정보(`page`, `perPageNum`)를 자동으로 수집합니다 <br>
   **3. 구간 데이터 조회 (MyBatis)** <br>
   &nbsp; `Criteria` 내부에서 계산된 시작 인덱스(`pageStart`)를 MySQL의 LIMIT 절에 대입하여 특정 구간의 레코드만 가져옵니다 <br>
   **4. 블록 계산 (PageMaker)** <br>
   &nbsp; 총 게시글 수를 기반으로 하단에 표시할 시작 페이지, 마지막 페이지, 이전/다음 버튼 활성화 여부를 계산합니다 <br>
   **5. JSON 응답 및 동적 렌더링** <br>
   &nbsp; 서버가 글 목록(`list`)과 페이징 데이터(`pageMaker`)를 `Map`에 담아 JSON으로 리턴하면, <br>
   &nbsp; jQuery가 이를 파싱하여 화면 테이블과 pagination UI를 동적으로 다시 그립니다 <br>
<br>


### 4-2. 핵심 컴포넌트별 상세 구현
**1. [Back-End] REST Controller & MyBatis SQL** <br>
- **API 설계 (GET /board/all)** <br>
   `@ResponseBody` 어노테이션을 사용하여 데이터 세트(list, pageMaker)를 JSON Object 포맷으로 클라이언트에 즉시 전달합니다 <br>
- **부분 조회 쿼리** <br>
    `LIMIT #{pageStart}, #{perPageNum}` 구문을 사용하여 대용량 테이블 환경에서도 필요한 행만 골라내므로 서버 메모리와 DataBase 부하를 최소화합니다 <br>
<br>

**2. [Front-End] Dynamic HTML Renderer (jQuery)** <br>
- **동기 이벤트 바인딩** <br>
동적으로 생성된 페이징 버튼(`.paginate_button a`)에 `on("click")` 이벤트를 위임(`Event Delegation`) 처리하여, 클릭 시 브라우저 기본 이동(`preventDefault()`)을 막고 가상 `<form>`의 page 값을 바인딩하여 `loadList()`를 재호출합니다 <br>
- **컴포넌트 갱신** <br>
서버가 응답한 JSON 데이터를 반복문(`$.each`) 돌려 리스트 뷰를 채우고, `makePagination()` 함수를 통해 하단 버튼 UI 구조를 완전히 초기화한 후 매번 새로 렌더링합니다 <br>
<br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/51c477dc-9964-47f9-a8b2-862806a71d12" width="90%" />
  <br>
   [비동기 RESTful 페이징 시스템]
</p>
<br>


## 5. 마라톤 완주 기록 검색 시스템 (Marathon Record Search)
사용자의 참가번호를 기반으로 마라톤 완주 기록을 정확하게 필터링하고, 동기식 폼(Form) 전송과 검색 조건 유지를 위한 가상 폼 파라미터 조작 기법을 적용한 개인 기록 조회 시스템입니다 <br>

### 5-1.시스템 아키텍처 및 연동 흐름 (Flow)
   **1. 검색 조건 입력 (JSP)** <br>
   &nbsp; 사용자가 특정 참가번호(mrNumber)를 입력하고 검색 버튼을 클릭합니다 <br>
   **2. 조건부 쿼리 빌드 (MyBatis XML)** <br>
   &nbsp; 동적 SQL 쿼리(`<sql>`, `<if>`) 레이어를 통해 검색 조건의 유무를 판단하고, 검색어가 존재할 경우에만 `WHERE` 절을 삽입하여 데이터 세트 및 전체 카운트를 추출합니다 <br>
   **3. 모델 적재 및 포워딩 (Controller)** <br>
   &nbsp; 페이징 계산기(`PageMaker`) 데이터와 레코드 리스트(`list`)를 `Model` 객체에 바인딩하여 결과 화면으로 포워딩합니다 <br>
   **4. 상태 유지 및 페이징 제어 (JavaScript/jQuery)** <br>
   &nbsp; 색어와 페이지 번호가 유실되지 않도록 하단 내비게이션 클릭 시 가상 폼(#pageFrm)에 페이지 번호를 주입하고, <br>
   &nbsp; 기존 검색 조건(type, keyword)을 결합하여 서버로 재전송합니다<br>
  
<br>

 
### 5-2. 핵심 기능 및 기술적 특징
#### 1) MyBatis 동적 SQL을 활용한 검색 최적화
 - **재사용 가능한 SQL 조각 (`<sql id="search">`)** <br>
     레코드 리스트를 추출하는 쿼리와 페이징 처리를 위한 전체 레코드 개수(COUNT(*))를 구하는 쿼리에서 검색 조건 처리 로직이 중복되는 것을 방지하기 위해 검색 쿼리를 분리하여 모듈화했습니다<br>
  - **동적 바인딩** <br>
    조건절 내부에서 MySQL의 `CONCAT(#{keyword})` 문법을 매핑하여 파라미터 유무에 따라 유연하게 대응하도록 설계했습니다<br>

#### 2) 검색 상태 유지를 위한 자바스크립트 가상 폼 조작
 - 검색 결과 화면에서 하단 페이징 버튼을 클릭할 때, 기존 검색 정보가 초기화되는 것을 막기 위해 동적 파라미터 전송 기법을 사용했습니다<br>
 - **SQL 조각 (`<sql id="search">`)** - 클릭한 대상의 `href` 속성에 담긴 페이지 번호를 필드(`#page`)에 대입한 후 `submit()`을 호출함으로써 검색 상태(`type`, `keyword`)와 페이징 상태가 안정적으로 유지됩니다<br>
<br>


 <p align="center">
    <img src="https://github.com/user-attachments/assets/2adb68b1-9131-4344-a547-4014c6c53c63" width="90%" />
    <br>
     [마라톤 완주 기록 검색 시스템]
  </p>
  <br>

  








