# 1. 서비스소개 
### 1-1. 서비스명
- Hankuk Marathon Online Platform
- Spring MVC 아키텍처 패턴을 기반으로 설계된 '마라톤 대회 안내 및 기록 조회 플랫폼<br>

<br>

### 1-2. 서비스설명
- 본 프로젝트는 Spring MVC 아키텍처 패턴을 기반으로 설계된 '마라톤 대회 안내 및 기록 조회 플랫폼 입니다 <br>
- Controller, Service, Mapper(Model) 계층을 분리하여 웹 애플리케이션의 유지보수성과 확장성을 높였습니다 <br>
- FAQ, 코스 정보, 공지사항 등 전반적인 안내 기능과 개인 기록 조회 서비스를 제공하여 사용자와의 소통을 지원합니다 <br>
- AJAX 비동기 통신 기술을 적용하여 화면 새로고침 없는 실시간 데이터 갱신을 구현함으로써 시스템 리소스를 최적화하고 사용자 편의성을 극대화했습니다 <br>
- Bootstrap 프레임워크를 활용하여 직관적이고 일관성 있는 UI를 구축했으며, 이를 통해 사용자 친화적인 웹 환경을 구현했습니다 <br>
<br>

### 1-3. 프로젝트기간
- 2025.07 ~ 2025.09 <br>
- renewal - 2026.06 <br>
<br>

### 1-4. 시연영상
https://www.youtube.com/watch?v=HDL472NQWXA

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
Spring MVC 구조와 MyBatis(Mapper)를 사용한 Spring MVC 기반 웹 애플리케이션
<p align="center">
  <img src="https://github.com/user-attachments/assets/4ee547ea-b48c-452f-90de-94a6e7d2aded" width="50%" />
  <br>
   [Spring Web MVC Framework]
</p>


# 4. DataBase E-R Diagram
<p align="center">
  <img src="https://github.com/user-attachments/assets/faa1f333-29c0-4f39-b495-8a8e97427c45" width=100% />
  <br>
  [E-R Diagram]
</p>
<br>



# 5. 기능구조도
<p align="center">
  <img src="https://github.com/user-attachments/assets/6f24b3d8-b193-43e0-9720-eaed34f23e1f" width=55% />
  <br>
  [기능구조도]
</p>
<br>




# 6. 페이지별 핵심기능가이드

<br>

## 1. 비동기 파일 업로드 시스템 (Asynchronous File Upload)
JSP Form 데이터와 이미지 파일을 jQuery FormData를 통해 비동기(AJAX)로 전송하고, 서버 측에서 Cos 라이브러리의 `MultipartRequest` 및 UUID를 활용하여 안전하게 파일을 서버 디렉토리에 저장하는 업로드 시스템입니다 <br>

<br>

### 1-1. 시스템 구성 및 흐름
   **1. 클라이언트 (JSP/HTML)** - `enctype="multipart/form-data"` 속성을 가진 <Form> 태그 구성 및 파일 선택 <br>
   **2. 비동기 요청 (JavaScript/AJAX)** - 자바스크립트 `FormData` 객체를 생성하여 멀티파트 데이터를 직렬화 없이 서버로 비동기 전송 <br>
   **3. 서버 파싱 (Controller)** - `MultipartRequest`를 이용하여 대용량 파일 저장 경로 및 최대 크기를 지정하고, 파일과 텍스트 파라미터를 분리 파싱 <br>
   **4. 고유 파일명 생성 (File Rename)** - 중복 방지를 위해 `UUID`를 생성하고, 기존 파일명과 결합하여 고유한 파일명으로 서버 디렉토리에 최종 저장 <br>
   **5. 정적 리소스 매핑 (Spring Web MVC)** - 업로드된 외부 디렉토리 경로를 웹 가상 경로로 매핑하여 클라이언트가 접근할 수 있도록 설정 <br>
<br>

### 1-2. 주요 구현 특징
- **FormData 기반의 Asynchronous 전송** <br>
  - 전체 페이지 새로고침없이 데이터를 전송하기 위해, 자바스크립트 내장 객체인 `FormData`를 활용하여 폼 데이터를 캡슐화합니다 <br>
  - AJAX 요청 시 `processData: false` 및 `contentType: false` 설정을 필수적으로 적용하여, 브라우저가 데이터를 쿼리 스트링으로 변환하거나 잘못된 Content-Type 헤더를 설정하는 것을 방지합니다 <br>
<br>

- **UUID 기반의 파일명 중복 방지 및 보안 강화** <br>
  - 동일한 파일명을 가진 사용자가 업로드할 경우 발생할 수 있는 파일 덮어쓰기 문제를 해결하기 위해 `UUID.randomUUID()`를 활용합니다 <br>
  - `DefaultFileRenamePolicy`로 1차 처리된 파일에 고유 서명(UUID)을 결합하여, 서버 물리 경로에 저장함으로써 데이터 무결성을 보장합니다 <br>
<br>

- **서버 디렉토리 자동 생성 기능** <br>
  - 서버 구동 중 해당 업로드 경로가 존재하지 않을 경우를 대비하여, `File.exists()` 검증을 거쳐 `mkdirs()` 메서드로 필요한 상위 디렉토리까지 안전하게 자동 생성하도록 예외 처리를 강화했습니다 <br>
<br>

- **외부 저장소 정적 웹 리소스 매핑** <br>
  - 웹 애플리케이션 내부(WAR)에 파일을 저장할 경우 재배포 시 파일이 삭제되는 문제를 방지하기 위해 외부 로컬 디렉토리를 지정했습니다 <br>
  - `servlet-context.xml` 설정을 통해 외부 물리 경로를 가상 웹 경로로 매핑함으로써, 보안을 유지하면서도 클라이언트 화면에 이미지를 정상적으로 렌더링할 수 있도록 지원합니다.<br>
<br>

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

<p align="center">
  <img src="https://github.com/user-attachments/assets/8e3c01da-994e-4388-98cc-77791b0c03c5" width="80%" />
  <br>
   [파일업로드 기능]
</p>
<br>





## 2. 비동기 RESTful 페이징 시스템 (Asynchronous Pagination System)
사용자가 요청한 페이지 번호에 맞춰 필요한 범위의 데이터만 DB에서 조회(Offset 기반 페이징)하고, 하단 페이지 블록을 동적으로 계산하여 화면 리로드 없이 페이징을 수행하는 고성능 비동기 시스템입니다 <br>
<br>

<br>

### 2-1. 시스템 구성 및 흐름
   **1. 페이지 요청 (Client)** - 사용자가 하단 페이지 번호를 클릭하면 자바스크립트가 `page` 변수 값을 갱신하고 AJAX 요청을 전송합니다 <br>
   **2. 파라미터 수집 (Controller)** - Spring MVC가 요청 파라미터를 `Criteria` 객체로 바인딩하여 현재 페이지 정보(`page`, `perPageNum`)를 자동으로 수집합니다 <br>
   **3. 구간 데이터 조회 (MyBatis)** - `Criteria` 내부에서 계산된 시작 인덱스(`pageStart`)를 MySQL의 LIMIT 절에 대입하여 특정 구간의 레코드만 가져옵니다 <br>
   **4. 블록 계산 (PageMaker)** - 총 게시글 수를 기반으로 하단에 표시할 시작 페이지, 마지막 페이지, 이전/다음 버튼 활성화 여부를 계산합니다 <br>
   **5. JSON 응답 및 동적 렌더링** - 서버가 글 목록(`list`)과 페이징 데이터(`pageMaker`)를 `Map`에 담아 JSON으로 리턴하면, jQuery가 이를 파싱하여 화면 테이블과 pagination UI를 동적으로 다시 그립니다 <br>
<br>


### 2-2. 핵심 컴포넌트별 상세 구현
   **1. [Back-End] REST Controller & MyBatis SQL** <br>
     - **API 설계 (GET /board/all)** - `@ResponseBody` 어노테이션을 사용하여 데이터 세트(list, pageMaker)를 JSON Object 포맷으로 클라이언트에 즉시 전달합니다 <br>
     - **부분 조회 쿼리** - `LIMIT #{pageStart}, #{perPageNum}` 구문을 사용하여 대용량 테이블 환경에서도 필요한 행만 골라내므로 서버 메모리와 DataBase 부하를 최소화합니다 <br>
   **[Front-End] Dynamic HTML Renderer (jQuery)** <br>
   - **동기 이벤트 바인딩** - 동적으로 생성된 페이징 버튼(`.paginate_button a`)에 `on("click")` 이벤트를 위임(Event Delegation) 처리하여, 클릭 시 브라우저 기본 이동(`preventDefault()`)을 막고 가상 form 태그의 page 값을 바인딩하여 `loadList()`를 재호출합니다 <br>
   - **컴포넌트 갱신** - 서버가 응답한 JSON 데이터를 반복문(`$.each`) 돌려 리스트 뷰를 채우고, `makePagination()` 함수를 통해 하단 버튼 UI 구조를 완전히 초기화한 후 매번 새로 렌더링합니다 <br>
<br>



<p align="center">
  <img src="https://github.com/user-attachments/assets/51c477dc-9964-47f9-a8b2-862806a71d12" width="90%" />
  <br>
   [비동기 RESTful 페이징 시스템]
</p>
<br>



## 3. AJAX 기반 비동기 공지사항 게시판
Bootstrap 3 UI 프레임워크와 jQuery 비동기 통신(AJAX)을 활용하여 구현한 공지사항 게시판입니다 <br>
전체 페이지의 새로고침없이 게시글의 생성, 조회, 수정, 삭제(CRUD)가 실시간으로 처리되며, 이미지 파일 유효성 검증 기능이 포함되어 있습니다 <br>
<br>

### 3-1. 웹 리로드 없는 비동기(AJAX) CRUD 및 RESTful 연동
- **전체 목록 조회** - 웹 페이지 로드 시 및 페이징 버튼 클릭 시 테이블 본문(#view)을 JSON 데이터를 기반으로 동적 재생성합니다 <br>
- **실시간 본문 상세 보기** - 게시글 제목 클릭 시 해당 글의 본문 영역(`<tr>`)을 슬라이드 토글 방식으로 확장하여 표시합니다 <br>
- **게시글 등록** - `FormData API`를 이용하여 텍스트 데이터와 대용량 이미지 파일을 동시에 비동기 전송합니다 <br>
- **인라인 수정** - 별도의 이동 없이, 테이블 내부에서 즉시 수정 가능한 (`<input>`, `<textarea>`) 폼으로 동적 전환됩니다 <br>
- **시글 삭제** - 데이터베이스 레코드 삭제 후, 리스트 가시 영역을 새로고침 없이 즉시 동기화합니다 <br>
<br>
  
### 3-2. 유연한 첨부파일 상태 관리 UI
- **조건부 컴포넌트 토글** - 수정 모드 진입 시, 기존 첨부파일의 유무에 따라 [기존 파일 삭제 버튼] 또는 [신규 파일 업로드 input]이 유기적으로 토글되어 화면에 표시됩니다 <br>
- **물리명 정제 로직** - 고유성 확보를 위해 `UUID`가 결합된 서버 측 파일명에서 문자열 파싱(`substring`, `indexOf`)을 적용하여, 사용자에게는 순수 원본 파일명만 정제하여 노출합니다 <br>
<br>

<p align="center">
    <img src="https://github.com/user-attachments/assets/5781f514-3fe0-45e8-a4ce-8cd5ce585da3" width="40%" />
    <br>
     [게시글수정]
  </p>

### 3-2. 션 기반 동적 UI 권한 관리
- JSTL `<c:if>` 문을 분기하여 세션 내 유저 객체(`mvo`)가 존재할 때만 글쓰기 버튼(`goForm()`) 및 입력 양식을 활성화합니다 <br>
- 자바스크립트 동적 UI 생성 구문 내에서 현재 로그인한 계정 아이디와 게시글 작성자의 소유권 아이디를 엄격히 비교하여, 일치하는 소유자에게만 [수정화면], [삭제] 버튼을 노출시킵니다 <br>
<br>

   <br>
    <p align="center">
      <img src="https://github.com/user-attachments/assets/ee4869b9-f860-41e0-9f6d-9c49b7c989f0" width="80%" />
    </p>

  <br>
  <p align="center">
    <img src="https://github.com/user-attachments/assets/8e75b958-b312-4794-81e0-636242f36c4f" width="40%" />
    <br>
     [공지사항 게시판]
  </p>
  




### 3-3. UUID 기반 파일 고유성 확보
- 동일한 파일명을 가진 데이터를 여러 사용자가 업로드할 경우 발생하는 데이터 덮어쓰기(Conflict) 문제를 해결하기 위해 `UUID.randomUUID()`를 적용했습니다 <br>
- **저장 구조** - UUID_원본파일명 형태로 저장하여 DB와 물리적 파일 간의 매핑을 안전하게 관리합니다 <br>
  <br>

### 3-4. 데이터 정합성 유지 및 효율적 수정 로직
- 파일이 새로 첨부되지 않으면` hidden`으로 넘겨받은 `originImgpath`를 유지하여 데이터 소실을 방지합니다. <br>
- 프로필 이미지 변경 시 File.delete()를 호출하여 서버 내 불필요한 구버전 파일을 물리적으로 삭제, 스토리지 용량을 효율적으로 관리합니다 <br>
  <br>    
  
### 3-5. 동적 디렉토리 생성
- 파일 저장 경로가 존재하지 않을 경우 `targetDir.exists()` 체크 후 `mkdirs()`를 통해 실행 시점에 디렉토리를 자동 생성하여 런타임 에러를 예방했습니다 <br>
<br>
 <p align="center">
    <img src="https://github.com/user-attachments/assets/1144f5a9-02e5-4b5a-8aad-8024d60540de" width="90%" />
    <br>
     [커뮤니티 - 첨부파일 업로드]
  </p>

<br>
 <p align="center">
    <img src="https://github.com/user-attachments/assets/6006a158-bdcd-4f9b-b7fb-2635a1a65ece" width="90%" />
    <br>
     [회원정보수정 - 프로필이미지 업로드]
  </p>

<br>


## 4. 마라톤 완주 기록 검색 시스템 (Marathon Record Search)
사용자의 참가번호를 기반으로 마라톤 완주 기록을 정확하게 필터링하고, 동기식 폼(Form) 전송과 검색 조건 유지를 위한 가상 폼 파라미터 조작 기법을 적용한 개인 기록 조회 시스템입니다 <br>

### 4-1.시스템 아키텍처 및 연동 흐름 (Flow)
   **1. 검색 조건 입력 (JSP)** - 사용자가 특정 참가번호(mrNumber)를 입력하고 검색 버튼을 클릭합니다 <br>
   **2. 조건부 쿼리 빌드 (MyBatis XML)** - 동적 SQL 쿼리(`<sql>`, `<if>`) 레이어를 통해 검색 조건의 유무를 판단하고, 검색어가 존재할 경우에만 `WHERE` 절을 삽입하여 데이터 세트 및 전체 카운트를 추출합니다 <br>
   **3. 모델 적재 및 포워딩 (Controller)** - 페이징 계산기(`PageMaker`) 데이터와 레코드 리스트(`list`)를 `Model` 객체에 바인딩하여 결과 화면으로 포워딩합니다 <br>
   **4. 상태 유지 및 페이징 제어 (JavaScript/jQuery)** - 색어와 페이지 번호가 유실되지 않도록 하단 내비게이션 클릭 시 가상 폼(#pageFrm)에 페이지 번호를 주입하고, 기존 검색 조건(type, keyword)을 결합하여 서버로 재전송합니다<br>
  
<br>

 
### 4-2. 핵심 기능 및 기술적 특징
#### 1) MyBatis 동적 SQL을 활용한 검색 최적화
 - **재사용 가능한 SQL 조각 (`<sql id="search">`)** - 레코드 리스트를 추출하는 쿼리와 페이징 처리를 위한 전체 레코드 개수(COUNT(*))를 구하는 쿼리에서 검색 조건 처리 로직이 중복되는 것을 방지하기 위해 검색 쿼리를 분리하여 모듈화했습니다<br>
  - **동적 바인딩** - 조건절 내부에서 MySQL의 `CONCAT(#{keyword})` 문법을 매핑하여 파라미터 유무에 따라 유연하게 대응하도록 설계했습니다<br>
<br>

#### 2) 검색 상태 유지를 위한 자바스크립트 가상 폼 조작
 - 검색 결과 화면에서 하단 페이징 버튼을 클릭할 때, 기존 검색 정보가 초기화되는 것을 막기 위해 동적 파라미터 전송 기법을 사용했습니다<br>
 - **재사용 가능한 SQL 조각 (`<sql id="search">`)** - `e.preventDefault()`를 통해 앵커(`<a>`) 태그의 기본 이동 동작을 제한하고, 클릭한 대상의 href 속성에 담긴 페이지 번호를 폼 내부의 숨겨진 필드(`#page`)에 대입한 후 submit()을 호출함으로써 검색 상태(`type`, `keyword`)와 페이징 상태가 안정적으로 유지됩니다<br>
<br>


 <p align="center">
    <img src="https://github.com/user-attachments/assets/2adb68b1-9131-4344-a547-4014c6c53c63" width="90%" />
    <br>
     [마라톤 완주 기록 검색 시스템]
  </p>
  <br>




## 5. 페이징 및 동적 검색 시스 (Paging & Dynamic Search)
대량의 데이터를 효율적으로 조회하고, 사용자가 원하는 정보를 빠르게 찾을 수 있도록 객체 지향적 페이징 처리와 MyBatis 동적 SQL 기반의 검색 기능을 구현했습니다 <br>

### 5-1. 주요 설계
 **Criteria** - 페이지 번호(`page`), 페이지당 게시글 수(`perPageNum`), 검색 키워드 및 타입 데이터를 하나의 객체로 캡슐화하여 계층 간 데이터 전달을 단순화했습니다 <br>
 **PageMaker** - 복잡한 페이징 연산(시작/끝 페이지 계산, 이전/다음 버튼 활성화 여부 등)을 전담하는 클래스를 설계하여 View의 로직 부담을 줄였습니다 <br>
 **Dynamic SQL** - MyBatis의 `<sql>`과 `<include>` 태그를 사용하여 검색 조건에 따라 SQL이 동적으로 생성되도록 구현, 유지보수성을 높였습니다 <br>
 <br>


### 5-2. MyBatis 동적 SQL과  태그 활용
 검색 조건에 따라 SQL이 유연하게 변하도록 <sql>과 <include> 태그를 사용했습니다. 이는 코드의 재사용성을 높이고 유지보수를 용이하게 합니다. <br>
 <br>
 <p align="center">
    <img src="https://github.com/user-attachments/assets/58a937b6-7edf-4dfa-8d5d-3f907d35148f" width="70%" />
    <br>
     [BoardMapper.xml]
  </p>
  <br>
  
### 5-3. JavaScript를 이용한 폼 컨트롤
 페이징 번호를 클릭했을 때 단순히 링크로 이동하는 것이 아니라, 숨겨진 폼(pageFrm)의 값을 JavaScript로 조작하여 전송합니다 <br>

 - **상태 유지** - 페이지 번호를 클릭해도 현재의 검색 조건(type, keyword)이 파라미터로 함께 전송되어 검색 결과 내에서 페이지 이동이 가능합니다 <br>
 - **상세보기 연동** - 게시글 제목 클릭 시에도 기존 페이징 정보를 파라미터로 들고 감으로써, '목록으로 돌아가기' 시 이전 상태를 복원합니다 <br>
 <p align="center">
    <img src="https://github.com/user-attachments/assets/d961aa71-5742-4140-accd-99cb74f03b9b" width="40%" />
    <br>
     [페이지 번호 클릭시 이동하기]
  </p>
 <br>

### 5-4. RedirectAttributes
수정/삭제 후 리다이렉트 시 `addAttribute`로 페이징 정보를 유지하고, `addFlashAttribute`로 결과 메시지를 일회성 모달로 띄워 사용자에게 명확한 피드백을 제공합니다 <br>
 <br>

 <p align="center">
    <img  src="https://github.com/user-attachments/assets/3e0cb289-b988-440b-8905-3c6c98cafb0c" width="90%" />
    <br>
     [페이징기능]
  </p>
 <br>


<p align="center">
    <img  src="https://github.com/user-attachments/assets/504760b9-e8f6-43af-853b-eaf419d77d6f" width="90%" />
    <br>
     [검색기능]
  </p>
 <br>



 ## 6. 계층형 답글 시스템
부모 게시글에 대한 답글을 계층적으로 시각화하고, 복잡한 정렬 순서를 유지하면서 파일 업로드와 페이징 상태까지 보존하는 고급 게시판 기능을 구현했습니다 <br>

### 6-1. 계층구조설계
  - **BoardGroup (그룹 번호)** - 원본 글(부모)과 그에 달린 모든 답글을 하나의 그룹으로 묶습니다. 부모글의 그룹 번호를 그대로 상속받습니다 <br>
  - **BoardSequence (순서)** -같은 그룹 내에서 위아래 출력 순서를 결정합니다. 부모글의 순서보다 1 큰 값을 가지며, 기존 답글들의 순서를 뒤로 밀어내는(Update) 로직을 포함합니다 <br>
  - **BoardLevel (들여쓰기)** - 답글의 깊이를 나타냅니다. 부모글의 레벨보다 1 큰 값을 가지며, UI에서 왼쪽 여백(Padding)을 결정하는 기준이 됩니다 <br>
<p align="center">
    <img  src="https://github.com/user-attachments/assets/3a770f2f-3b21-4350-9c48-c0124039bd66" width="50%" />
    <br>
     [계층구조]
  </p>
 <br>

### 6-2. 핵심 로직: 답글 저장 프로세스
  - **1. 부모 정보 조회** - 답글을 달 대상(부모)의 `idx`를 통해 부모의 `Group`, `Sequence`, `Level` 정보를 가져옵니다 <br>
  - **2. 시퀀스 재배열 (Update):** -같은 그룹 내에서 부모보다 아래에 있던 기존 답글들의 `Sequence`를 모두 1씩 증가시켜 새로운 답글이 들어갈 자리를 만듭니다 <br>
  - **데이터 삽입 (Insert)** - 모의 정보를 바탕으로 계산된 신규 `Sequence`, `Level` 값을 적용하여 저장합니다 <br>
 <br>
 

<p align="center">
    <img  src="https://github.com/user-attachments/assets/a0a41807-9ae5-439e-9e1d-dc4cdec1bd8f" width="90%" />
    <br>
     [답글기능]
  </p>
 <br>



## 6. 비동기 댓글 기능 
페이지 새로고침 없이 실시간으로 댓글 데이터를 처리하는 비동기 통신 시스템을 구현했습니다 <br>

### 6-1. RESTful 기반 비동기 로드 (AJAX)
 - 게시글 고유 번호(idx)를 파라미터로 전달하여 해당 게시글에 속한 댓글 리스트를 JSON 형태로 수신합니다 <br>
 - 전체 페이지를 다시 읽어오지 않고 댓글 영역만 동적으로 렌더링하여 서버 부하를 줄이고 사용자 체감 속도를 개선했습니다 <br>
 
### 6-2. 효율적인 데이터 직렬화 및 실시간 갱신
 - jQuery의 serialize() 메서드를 사용하여 폼 내의 다수 입력 필드를 쿼리 스트링으로 자동 변환, 데이터 전송 로직을 간결화했습니다 <br>
 - 댓글 등록 성공 시 콜백 함수를 통해 목록 로드 함수(loadCmt)를 재실행함으로써 별도의 액션 없이도 최신 데이터가 반영되도록 설계했습니다 <br>

### 6-3. 데이터 무결성을 위한 논리적 삭제(Soft Delete)
 - DB에서 데이터를 물리적으로 즉시 삭제하는 대신, cmtAvailable 플래그(0: 삭제, 1: 활성)를 업데이트하는 방식을 채택했습니다 <br>
 - 삭제된 데이터도 "작성자에 의해 삭제된 댓글입니다"라는 메시지를 남겨 게시글의 전체적인 맥락과 대화 흐름이 깨지지 않도록 처리했습니다 <br>

### 6-4. 세션 기반 사용자 권한 검증
 - 세션에 저장된 사용자 정보(`memID`)와 댓글 작성자 정보를 대조하여, 본인의 댓글에만 삭제 버튼이 활성화되도록 프론트엔드와 백엔드에서 이중으로 검증합니다 <br>

   
 <p align="center">
    <img  src="https://github.com/user-attachments/assets/680c6016-3d93-435a-aed0-336b77dd7be6" width="70%" />
    <br>
     [댓글기능]
  </p>
 <br>

<p align="center">
    <img  src="https://github.com/user-attachments/assets/629cfe92-52ff-4ce0-9c6b-64c86cd5b0e5" width="70%" />
    <br>
     [댓글기능 - 논리적 삭제]
  </p>
 <br>


## 7. 비동기 좋아요 기능 
- 사용자가 게시물에 대해 '좋아요'를 누르거나 취소할 수 있는 비동기 시스템입니다 <br>
- 페이지 새로고침 없이 실시간으로 상태가 반영되도록 구현했습니다 <br>

### 7-1. 주요 기능
 - **비동기 처리** - `AJAX`를 사용하여 화면 전환 없이 좋아요 수와 버튼 상태를 업데이트합니다 <br>
 - **상태 유지** - `CM_LIKE` 테이블에사용자별 좋아요 기록을 저장하여 재방문 시에도 상태를 유지합니다 <br>
 - **토글 방식** - 하나의 버튼으로 '좋아요'와 '좋아요 취소' 기능을 번갈아 수행합니다

### 7-2. 프로세스 흐름 <br>

#### 1) 좋아요 클릭시 (likePlus) <br>
 - `CM_BOARD`의 `LIKECOUNT`를 +1 합니다 <br>
 - `CM_LIKE` 테이블에 사용자 ID와 게시물 번호를 `INSERT` 합니다 <br>
 - 성공 시 버튼의 색상을 `btn-danger`로 변경을 합니다 <br>
 
#### 2) 좋아요 취소 시 (unLike) <br>
 - `CM_BOARD`의 `LIKECOUNT`를 -1 합니다 <br>
 - `CM_LIKE` 테이블에 사용자 ID와 게시물 번호를 `DELETE` 합니다 <br>
 - 성공 시 버튼의 색상을 `btn-default`로 초기화 합니다 <br>

 <p align="center">
    <img src="https://github.com/user-attachments/assets/c9bcb55e-f0b1-4748-857c-712a0af70f3e"" width="90%" />
    <br>
     [좋아요 기능]
  </p>
 <br>


## 8. 공통 프로필 상세 정보 시스템
- 게시글 리스트나 댓글 창에서 작성자의 이름을 클릭하면, 비동기 통신을 통해 해당 사용자의 상세 정보를 모달 형태로 제공하는 기능입니다 <br>
- 페이지 새로고침 없이 실시간으로 상태가 반영되도록 구현했습니다 <br>
  <br>
  
 ### 주요 특징
 - **컴포넌트 재사용성** - 게시글, 댓글, 쪽지함 등 사용자 ID가 노출되는 모든 곳에 동일한 로직을 적용하여 코드 중복을 최소화했습니다 <br>
 - **이벤트 위임** - `$(document).on("click", ...)` 방식을 사용하여, 페이지 로드 후 AJAX로 동적으로 생성된 댓글 작성자 클릭 시에도 이벤트가 안정적으로 작동합니다 <br>
 - **비동기 데이터 바인딩** - AJAX를 통해 서버로부터 회원 정보를 JSON 형태로 받아와 모달에 실시간으로 반영합니다 <br>
 <br>
 
 ### 기술적 포인트
 - **Data Attribute 활용** - HTML data-* 속성에 작성자 ID를 숨겨 저장하고, 이를 서버 통신용 키(Key)값으로 활용합니다. <br>
 - 모달 내부 데이터 저장소(`data("writer_ID")`)에 실제 ID를 보관하여, 화면 표시 내용과 별개로 '메시지 보내기' 등 내부 로직에서 정확한 참조가 가능하도록 설계했습니다 <br>

<p align="center">
    <img src="https://github.com/user-attachments/assets/5bd63012-cbe3-4a24-a07f-59fc6c1e1681" width="40%" />
    <br>
     [프로필 상세정보 시스템 - Data Attribute 활용]
  </p>
 <br>
 <p align="center">
    <img src="https://github.com/user-attachments/assets/5da944e2-9f4e-4209-8e91-c1af35b3f9ae" width="90%" />
    <br>
     [프로필 상세정보 시스템]
  </p>
 <br>


## 9. 실시간 그룹 오픈채팅 시스템 (WebSocket) <br>
### 9-1. 주요 특징 (Key Features) <br>
- **전이중 통신(Full-Duplex)** - 표준 `Native WebSocket API` 활용하여 서버와 클라이언트 간의 실시간 양방향 메시지 전송을 구현 <br>
- **그룹별 세션 관리** - 같은 그룹에 속한 사용자들끼리만 메시지를 주고받을 수 있도록 세션 필터링 로직을 설계 <br>
- **실시간 접속자 리스트** - `ServletContext`를 활용해 서버 전체의 접속자 현황을 관리하고, 사용자의 입장/퇴장 시 실시간으로 접속자 목록이 갱신되도록 구현<br>
- **Handshake 인터셉터** - `HttpSessionHandshakeInterceptor`를 통해 HTTP 세션 정보(로그인 아이디, 그룹명 등)를 웹소켓 세션으로 안전하게 전이시켜 활용<br>
<br>

### 9-2. [Server] 세션 핸들링 및 메시지 브로드캐스팅 <br>
- **접속 관리** - 사용자가 연결되면 `ArrayList<WebSocketSession>`에 저장하고, 입장 메시지를 동일 그룹 사용자들에게 전달한다 <br>
- **메시지 분기 처리** - 메시지 페이로드의 특정 접두사(`#$nickName_`)를 분석하여 입장 알림인지, 일반 채팅 메시지인지 판별하여 처리한다 <br>
- **비정상 종료 대응** - 브라우저 닫기나 네트워크 단절 시 `afterConnectionClosed`를 통해 세션을 즉시 제거하고 퇴장 알림을 보냄으로써 세션 누수를 방지한다 <br>
<br>

### 9-3. [Client] 동적 UI 및 웹소켓 이벤트 처리 <br>
- **이벤트 리스너** - onopen, onmessage, onclose 이벤트를 각각 정의하여 서버와의 연결 상태에 따른 UI 변화를 구현 <br>
- **조건부 렌더링** -서버로부터 받은 메시지가 '나'인 경우와 '타인'인 경우를 구분하여 말풍선 위치와 스타일을 다르게 렌더링 <br>


<p align="center">
  <img src="https://github.com/user-attachments/assets/ea39aba7-77a1-4641-9e74-d64c6aaad9ae" width=90% />
  <br>
  [실시간 그룹 오픈채팅 기능]
</p>

<br>



