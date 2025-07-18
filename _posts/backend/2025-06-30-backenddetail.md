---
title: "backend 주요기능 구현상세"
date: 2025-06-30
categories: [backend]
permalink: /backend/backenddetail/
---

### 🛠️ **Backend 기능 체크리스트 (Spring Boot MSA)**

**Spring Boot 3 기반 마이크로서비스 구조 (REST + Kafka + JWT + Redis)**

---

#### ✅ 인증 및 보안 처리

* [ ] **회원가입 (이메일 + 비밀번호 암호화)**
* 사용자가 이메일과 비밀번호로 가입하며, BCrypt로 비밀번호를 암호화하여 저장

* [ ] **이메일 인증 + Redis 코드 저장**
* 인증용 6자리 숫자 코드를 생성하여 Redis에 저장하고 이메일 전송

* [ ] **인증 코드 재전송 제한 (5분 이내 차단)**
* 인증 요청 시 5분 내 중복 요청 제한 → Redis TTL 기반 제어

* [ ] **로그인 (JWT Access/Refresh 발급)**
* 이메일/비밀번호로 로그인 시 JWT 토큰 2종 발급 (Access: 15분, Refresh: 7일)

* [ ] **Refresh 토큰 기반 재로그인**
* 만료된 AccessToken을 RefreshToken으로 자동 갱신

* [ ] **비밀번호 재설정 API (인증코드 기반)**
* 이메일 인증코드를 통해 새로운 비밀번호 설정 가능

* [ ] **로그인 10회 이상 실패 시 24시간 제한**
* Redis를 활용하여 실패 횟수 추적 후 로그인 제한 적용

* [ ] **블랙리스트 IP 관리**
* 악성 IP의 로그인 또는 인증 요청 차단 기능 포함

* [ ] **Spring Security 기반 JWT 필터 인증 적용**
* 인증된 요청만 API 접근 가능하며, 필터에서 JWT 검증 수행

* [ ] **HttpOnly + Secure 쿠키 기반 RefreshToken 처리**
* 토큰 보안을 위해 RefreshToken은 쿠키에 저장

---

#### 👤 사용자 정보 및 관심 항목 관리

* [ ] **사용자 프로필 CRUD**
* 닉네임, 지역, 연락처, 관심기관/지역/공사 등 사용자 정보 조회 및 수정

* [ ] **관심기관/지역/공사 CRUD**
* 사용자의 관심 항목 등록/수정/삭제 기능 제공

* [ ] **관심 정보 변경 시 Kafka 이벤트 발행**
* AI 서버 또는 데이터 수집 서버에 전달될 수 있도록 이벤트 처리

---

#### 📝 게시글 도메인 처리

* [ ] **게시글 등록, 조회, 삭제**
* 게시판 기본 기능 제공 (추후 댓글, 좋아요 등 확장 가능)

* [ ] **X-User-Id 기반 인증 헤더 처리**
* API 호출 시 사용자 식별을 위해 사용자 ID 포함

---

#### 📦 마이크로서비스 통신 및 이벤트 처리

* [ ] **Kafka 이벤트 발행 (회원가입 시 AI 서버 통보 등)**
* 회원가입, 관심정보 변경 등 주요 이벤트를 Kafka로 전송

* [ ] **Kafka 이벤트 수신 (추후 분석 결과 수신용)**
* FastAPI 서버 또는 외부 분석 서버의 응답 수신 처리 예정

---

#### 📡 API 문서화 및 예외 처리

* [ ] **Swagger 기반 자동 API 문서화**
* Swagger-UI를 통해 REST API 테스트 및 문서 자동화

* [ ] **공통 응답 포맷 적용 (BaseResponse)**
* success, data, errors 포맷으로 모든 응답 일관성 유지

* [ ] **Global Exception Handler로 예외 통합 처리**
* 공통된 에러 메시지 형식으로 에러 응답 반환

---

#### 🔐 보안 및 운영 고려사항

* [ ] **CORS 정책 설정**
* 프론트엔드 서버에서만 접근 가능하도록 도메인 제한

* [ ] **Actuator 보안 설정**
* /actuator 경로는 인증된 관리자만 접근 가능하게 설정

* [ ] **Redis + Kafka + MySQL 보안**
* 내부 Private IP로만 통신 가능하도록 제한

* [ ] **JWT 클레임 확장 처리 (Role 등)**
* 사용자 권한을 JWT에 포함하여 관리자/일반 사용자 구분

* [ ] **Sentry 또는 로그 기반 감사 기능 (선택사항)**
* 에러 추적 및 주요 사용자 행동 감사 로그 기록 가능

---

#### 🧪 테스트 및 CI/CD 대응

* [ ] **단위 테스트 / 통합 테스트 작성**
* JUnit + MockMvc 또는 WebTestClient 사용

* [ ] **GitHub Actions 기반 CI 스크립트 연동**
* PR → Build → Docker 이미지 → Deploy 자동화

* [ ] **Swagger, Test 코드 분리된 빌드 적용**
* 운영 환경에서 테스트 코드 제외 빌드 전략 구성

---

---

### 🛡️ 보안 항목 고도화 제안

#### ✅ API 보호 및 인증 강화

* [ ] **JWT 서명 알고리즘 보안 강화**
* HS256 대신 공개키 기반의 RS256 서명 알고리즘으로 위조 방지 강화

* [ ] **AccessToken 재사용 방지 처리**
* 로그아웃 시 해당 토큰 블랙리스트 처리 또는 jti 클레임 적용

* [ ] **인증 관련 감사 로그 저장**
* 로그인/인증/비밀번호 변경 등 민감 활동에 대한 감사 기록 남김

---

#### ✅ Redis / DB / Kafka 보안 강화

* [ ] **Redis requirepass 설정**
* 외부 접근 차단 및 비밀번호 인증 적용

* [ ] **MySQL 접근 권한 최소화 및 계정 분리**
* 읽기/쓰기 권한을 구분하고 관리자 계정 별도 운영

* [ ] **Kafka 인증 및 ACL 설정**
* SASL 인증 및 토픽 별 읽기/쓰기 권한 제어

---

#### ✅ Spring Security 고도화

* [ ] **CSRF 보호 정책 명확화**
* REST API 전용 서비스는 비활성화, Web Form 기반은 활성화

* [ ] **보안 헤더 추가 설정**
* Content-Security-Policy, X-Frame-Options 등 설정

* [ ] **Rate Limiting (IP 기반 요청 제한)**
* 인증, 민감 API에 IP 기반 요청 횟수 제한 적용

---

#### ✅ 개인정보 및 민감 정보 보호

* [ ] **API 및 로그에서 개인정보 마스킹**
* 이메일, 연락처 등의 정보는 일부만 출력

* [ ] **개인정보 저장 시 암호화 적용**
* AES-256 등으로 DB 내 민감 필드 암호화 적용 가능






