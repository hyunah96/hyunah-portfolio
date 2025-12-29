### 🔗Link

https://github.com/hyunah96/Story 

### 프로젝트 배경

![메인화면](./img/main.png)
결혼 준비 과정에서는 정보가 공급자 중심으로 제공되는 경우가 많아 정보 비대칭이 발생하고, <br>그 결과 원하는 조건의 결혼식을 찾기까지 많은 시행착오를 겪곤 합니다.  
Wellding은 사용자가 다양한 업체를 직접 비교, 탐색하고, 후기 확인부터 견적 요청, 결제까지 전 과정을 <br>하나의 플랫폼에서 처리할 수 있도록 기획한 웨딩 통합 서비스입니다.

### 웨딩 통합 플랫폼, Wellding

**1. 원하는 웨딩홀을 조건별로 빠르게 탐색**  
날짜, 예산, 조건에 맞춰 웨딩홀을 검색하고, 리스트와 상세페이지를 통해 정보를 비교할 수 있습니다. <br>또한 동일 업체의 다른 홀까지 함께 확인하며 최적의 선택을 돕습니다.

**2. 스튜디오·드레스·메이크업(SDM)을 한 곳에서 비교**  
스튜디오/드레스/메이크업을 카테고리별로 탐색할 수 있으며, 추천 콘텐츠를 통해 선택 과정의 부담을 줄일 수 있습니다.

**3. 후기 기반 커뮤니티**  
리뷰 게시판의 별점 기능으로 만족도를 한눈에 파악할 수 있고, 노하우 공유 및 댓글 기능을 통해<br> 실제 경험 기반의 정보를 얻을 수 있습니다.

**4. 마이페이지에서 일정,결제,혜택을 통합 관리**  
결혼 예정일 변경, 장바구니 상품 관리, 결제/취소 내역 조회가 가능하며 쿠폰/포인트 적용과<br> 카카오페이 결제까지 지원합니다. 또한 회원정보와 쿠폰 현황을 한 곳에서 관리할 수 있습니다.

**5. 사용자/관리자 분리 운영**  
사용자는 로그인/회원가입/비밀번호 찾기(임시비밀번호 이메일 발송)를 지원하며, <br>관리자는 회원/상품(SDM·홀)/게시판/공지·이벤트/결제 및 취소 요청 승인등 운영 전반을 관리할 수 있습니다.

### 사용 기술 및 개발 환경
- 개발언어 : JAVA, JSP, JQUERY, HTML5, CSS3, XML
- 서버 : ORACLE, APACHE TOMCAT, MYBATIS, MAVEN, NGROK
- 개발도구 : SPRING, ECLIPSE, BOOTSTRAP, SWEET ALERT2, JAVAX EMAIL, COLORBOX, QRCODE.js, SLICKSLIDE, CHANNEL.io, AJAX
- IDE : eclipse
- 형상관리 : GIT, GIT HUB, SOURCETREE
- API : KakaoPay, KakaoMap

### STEP1. 상세 페이지

<p>
  <img src="./img/login-merge.png" alt="회원가입" width="90%">
</p>
<p>
  <img src="./img/id_pw.png" alt="아이디 비밀번호 찾기" width="90%">
</p>
<p>
  <img src="./img/notice.png" alt="공지사항" width="48%">
  <img src="./img/location.png" alt="위치" width="48%">
</p>
<p>
  <img src="./img/weddingHall.png" alt="웨딩홀목록1" width="48%">
  <img src="./img/weddingHall2.png" alt="웨딩홀목록2" width="48%">
</p>

<p>
  <img src="./img/planner.png" alt="플래너1" width="48%">
  <img src="./img/planner2.png" alt="플래너2" width="48%">
</p>

<p>
  <img src="./img/studio.png" alt="스튜디오1" width="48%">
  <img src="./img/studio2.png" alt="스튜디오2" width="48%">
</p>
<p>
  <img src="./img/dress.png" alt="dress" width="48%">
  <img src="./img/dress2.png" alt="dress2" width="48%">
</p>
<p>
  <img src="./img/makeup.png" alt="메이크업" width="48%">
  <img src="./img/basket.png" alt="장바구니" width="48%">
</p>
<p>
  <img src="./img/pay.png" alt="결제" width="48%">
  <img src="./img/pay2.png" alt="결제2" width="48%">
</p>
<p>
  <img src="./img/review.png" alt="업체리뷰" width="48%">
  <img src="./img/knowhow.png" alt="노하우공유" width="48%">
</p>
<p>
  <img src="./img/cupon.png" alt="쿠폰1" width="48%">
  <img src="./img/cupon2.png" alt="쿠폰2" width="48%">
</p>

### 🔗kakaopay API

https://developers.kakaopay.com/docs/getting-started/api-common-guide/restapi

### STEP2. DataBase ERD

### STEP3. MVC 패턴 적용
페이지를 `Model`, `View`, `Controller`로 분리하여 단위 별로 의존성을 줄이려고 노력했습니다.