# 개인용 생명과학 15분 학습 웹서비스
## 최종 MVP 제품 요구사항·개발 계획 v2

- 기준 문서: `personal_biology_15min_learning_handoff_v2.md`
- 문서 성격: Phase 0 최종 산출물
- 제품 범위: 개인용·단일 사용자·비상용 MVP
- 핵심 우선순위: 실제 학습 지속성 > 기능 수 > 기술적 확장성

## 문서 역할과 우선순위

- [`../CODEX_MASTER.md`](../CODEX_MASTER.md): 모든 Phase에 적용되는 공통 규칙
- 이 문서: 제품 요구사항, 사용자 흐름, 화면, 데이터 및 콘텐츠 구조의 상세 기준
- [`codex-phase-prompts.md`](codex-phase-prompts.md): Phase 0~7 실행 범위와 절차의 기준
- [`../README.md`](../README.md): 사람을 위한 짧은 프로젝트 소개
- [`decisions.md`](decisions.md): 주요 제품·기술 결정과 이유

공통 제약이 충돌하면 `CODEX_MASTER.md`를 우선하고, 제품 세부사항은 이 문서, 해당 Phase의
실행 범위는 `codex-phase-prompts.md`를 따른다. 아래 11절의 프롬프트 사본보다
`codex-phase-prompts.md`가 우선하며, 사본은 추후 별도 정리 대상이다.

---

# 프로젝트 실행 환경 확정

- 프로젝트명: bioStudy
- GitHub 저장소: `https://github.com/wanted1213/bioStudy.git`
- 기본 브랜치: `main`
- 배포: Vercel
- 배포 방식: GitHub 저장소를 Vercel 프로젝트에 연결하여 `main` push 시 자동 배포
- 사용자: 서비스 소유자 한 명
- 기본 시간대: `Asia/Seoul`
- 패키지 매니저: 저장소에 lockfile이 없으면 npm 사용
- 기본 기술 스택: Next.js App Router + TypeScript strict
- 데이터 저장: MVP 전체 localStorage
- 콘텐츠 저장: 저장소 내 JSON 파일
- 서버 DB·로그인·다중 사용자 기능은 도입하지 않음

## Vercel 적용 원칙

- Phase 1부터 항상 `npm run build`가 성공해야 한다.
- 브라우저 전용 API인 localStorage는 Client Component 또는 클라이언트 경계 안에서만 사용한다.
- 빌드 중 `window`, `localStorage`, 시간대 처리 때문에 오류가 나지 않도록 한다.
- Vercel 배포를 위해 불필요한 서버, Docker, 별도 인프라를 추가하지 않는다.
- Phase 1 완료 후 사용자가 GitHub 저장소를 Vercel에 연결해 최초 배포한다.
- 이후 각 Phase 완료 시 로컬 검증과 Vercel 배포 상태를 분리해 확인한다.
- OpenAI API를 사용하는 Phase 6 전까지 Vercel 환경변수는 필수가 아니다.

---

# 1. 검토 결론

기준 문서의 방향은 일관되고, 커리큘럼 수량도 정확하다.

- 12개 모듈
- 새 개념 96회
- 복습·통합 24회
- 총 120회
- 주 5회 사용 시 약 24주

MVP의 핵심은 다음 한 문장으로 확정한다.

> 사용자가 사이트를 열면 그날 완료하지 않은 학습 세션이 즉시 이어지고, 복습·오늘의 학습·문제·직접 설명을 약 15분 안에 마친 뒤 진도와 다음 복습 일정이 로컬에 저장되는 개인용 학습 도구.

상용 플랫폼, 다중 사용자, 게임화, 코딩 실습, 신약개발 별도 과정은 MVP에 포함하지 않는다.

---

# 2. 모호하거나 충돌했던 부분과 최종 결정

## 2.1 매일의 복습과 24개의 복습·통합 세션

두 종류를 분리한다.

1. **일일 복습(Daily Review)**
   - 과거에 배운 개념 중 복습 예정일이 된 항목을 세션 시작 시 문제로 다시 확인한다.
   - 기본 3문제, 최대 5문제다.
   - 커리큘럼 120회 수에 별도로 더해지지 않는다.

2. **커리큘럼 복습·통합 단위(Integration Unit)**
   - 120회 안에 포함된 24개 정규 학습 단위다.
   - 새 개념을 추가하기보다 한 모듈의 기존 개념을 연결하고 통합한다.

따라서 실제 하루 세션은 항상 `일일 복습 + 오늘의 커리큘럼 단위 + 확인 문제 + 직접 설명` 구조다. 오늘의 커리큘럼 단위는 새 개념일 수도 있고 복습·통합 단위일 수도 있다.

## 2.2 “오늘의 새 개념”과 복습·통합일의 충돌

화면과 데이터 명칭을 **오늘의 학습(Today Unit)** 으로 통일한다.

- 개념 단위일: 새 개념 설명을 제공한다.
- 통합 단위일: 새 사실을 늘리기보다 연결 설명, 비교, 전체 흐름을 제공한다.

## 2.3 접속 즉시 시작의 의미

루트 경로 `/`가 오늘 학습 화면이다.

- 오늘 미완료 세션이 있으면 마지막 저장 단계부터 즉시 재개한다.
- 오늘 세션이 없으면 자동으로 생성하고 첫 단계부터 보여준다.
- 오늘 완료했다면 다음 학습을 자동으로 열지 않고 완료 화면을 보여준다.
- 진도 화면은 보조 화면이며 첫 진입 화면이 아니다.

## 2.4 학습하지 못한 날 처리

달력 날짜에 맞춰 커리큘럼을 건너뛰지 않는다.

- 다음 학습 단위는 항상 `가장 앞에 있는 미완료 단위`다.
- 결석한 날의 세션을 여러 개 쌓아두지 않는다.
- 하루에 자동 제공되는 새 커리큘럼 단위는 하나뿐이다.
- 같은 날 완료 후 다음 단위를 자동으로 시작하지 않는다.

## 2.5 직접 설명과 AI 피드백

직접 설명 단계는 Phase 1부터 존재해야 한다. AI는 필요조건이 아니다.

초기 동작:

1. 사용자가 짧은 설명을 입력한다.
2. 저장 후 승인된 콘텐츠의 핵심 확인 항목을 보여준다.
3. 사용자가 `대체로 설명함` 또는 `다시 복습 필요`를 직접 선택한다.
4. 직접 평가는 복습 우선순위에 참고할 수 있지만, 객관식 정답처럼 숙달을 자동 확정하지 않는다.

Phase 6의 AI 피드백은 선택적 보조 기능이며, AI 실패·미설정 상태에서도 학습 완료가 가능해야 한다. AI는 정답 기준이나 MASTERED 상태를 결정하지 않는다.

## 2.6 localStorage와 DB

MVP는 **끝까지 localStorage를 기본 저장소로 유지**한다.

이유:

- 사용자는 한 명이다.
- 로그인과 서버 운영이 불필요하다.
- 실제 학습 시작을 가장 빠르게 할 수 있다.
- Phase 7의 내보내기·가져오기로 백업 위험을 줄일 수 있다.

SQLite, Supabase, ORM은 다음 조건이 실제로 생길 때만 별도 결정한다.

- 여러 기기 동기화가 반드시 필요함
- 브라우저 데이터 손실이 반복됨
- 서버 기반 AI 로그나 장기 분석이 실제로 필요함

현재 Phase 계획에는 DB 도입을 넣지 않는다.

## 2.7 콘텐츠 형식

MVP의 기준 형식은 **JSON**으로 확정한다.

- 120개 단위의 순서, ID, 문제 정답, 상태를 구조적으로 검증하기 쉽다.
- Markdown 본문을 별도로 파싱하는 복잡도를 피한다.
- 설명에 복잡한 서식이 필요하지 않다.

본문에 필요한 서식은 문단과 간단한 목록 정도로 제한한다. 향후 정말 필요할 때만 Markdown 필드를 검토한다.

## 2.8 콘텐츠 승인과 앱 노출

- `DRAFT`: 초안
- `REVIEWED`: 사람이 1차 검토했으나 배포 승인 전
- `APPROVED`: 앱에서 학습용으로 노출 가능
- `RETIRED`: 더 이상 노출하지 않음

프로덕션 학습 경로에는 `APPROVED`만 노출한다.

Codex나 AI가 새로 작성한 과학 콘텐츠는 기본적으로 `DRAFT`여야 한다. 구조 검증 통과는 과학적 정확성 승인을 뜻하지 않는다. 상태를 `APPROVED`로 바꾸는 것은 별도의 사람 검토 작업이다.

## 2.9 출처 2개 원칙

핵심 과학 사실은 원칙적으로 서로 독립적인 신뢰 자료 2개 이상으로 확인한다.

권장 조합:

- 표준 교재 또는 OpenStax
- 정부·공공기관 자료(NCBI, NIH 등)
- 대학 교육 자료
- 신뢰할 수 있는 리뷰 논문 또는 전문 참고서

같은 원문을 재인용한 두 웹페이지는 독립 출처 2개로 계산하지 않는다.

## 2.10 초기 샘플 범위

기준 문서의 “샘플 모듈 3~5개”와 “Module 1의 개념 3개” 중 후자를 채택한다.

- 개발 검증용 실제 범위: Module 1의 개념 단위 3개 + 통합 단위 1개에 해당하는 테스트/미리보기 데이터
- 테스트 fixture는 프로덕션 승인 콘텐츠로 간주하지 않는다.
- Codex는 과학 검토 없이 이 파일들을 `APPROVED`로 변경하지 않는다.

## 2.11 연속 학습 일수

- 기준 시간대: `Asia/Seoul`
- 한 날짜에 완료된 세션이 하나 이상 있으면 학습일로 계산한다.
- 연속된 현지 날짜의 완료 기록으로 streak를 계산한다.
- 통계 값은 완료 세션에서 파생하고 중복 저장하지 않는 것을 원칙으로 한다.

## 2.12 문제 유형 도입 순서

- Phase 1: 객관식만 구현
- Phase 2: 객관식, O/X, 빈칸, 짧은 단답형의 스키마와 채점 규칙 구현
- 서술형 직접 설명: 별도 단계로 Phase 1부터 저장하되 자동 정답 처리하지 않음
- 순서 배열·개념 연결 문제: MVP 제외

---

# 3. 최종 MVP 제품 요구사항 문서

## 3.1 제품 목표

1. 사용자가 매일 무엇을 공부할지 선택하지 않아도 된다.
2. 사이트를 열자마자 오늘 학습을 시작하거나 이어갈 수 있다.
3. 한 번의 세션이 약 15분 안에 끝난다.
4. 이전 개념을 간단한 규칙으로 반복한다.
5. 120개 커리큘럼 단위를 순서대로 누적한다.
6. 승인된 콘텐츠만 실제 학습에 사용한다.
7. 개발과 기능 확장이 학습 자체를 방해하지 않게 한다.

## 3.2 대상 사용자

- 사용자 1명: 서비스 소유자 본인
- 한국어 설명을 기본으로 사용
- 생명과학 입문자이지만 개발 경험은 있음
- 생명과학 문해력과 바이오 데이터 흐름 이해가 목표

## 3.3 필수 기능 요구사항

### FR-01 오늘 학습 자동 진입

루트 경로에서 오늘 세션을 자동 생성하거나 기존 미완료 세션을 복원한다.

### FR-02 일일 복습

복습 예정 개념 중 우선순위가 높은 항목을 기본 3개, 최대 5개 문제로 제공한다. 복습 대상이 없으면 해당 단계를 짧게 건너뛴다.

### FR-03 오늘의 커리큘럼 단위

가장 앞의 미완료 승인 단위 하나를 제공한다. 개념 단위 또는 통합 단위다.

### FR-04 이해 확인 문제

오늘 단위와 연결된 2~4개의 문제를 제공한다. 문제 유형은 단계적으로 지원한다.

### FR-05 직접 설명

사용자가 배운 내용을 짧게 직접 설명하고, 승인된 핵심 항목을 보며 스스로 확인한다.

### FR-06 완료 처리

전체 필수 단계를 마치면 해당 날짜와 커리큘럼 단위를 완료 처리하고, 개념별 다음 복습일을 저장한다.

### FR-07 중단 후 재개

새로고침하거나 브라우저를 닫아도 오늘 세션의 현재 단계와 답변을 복원한다.

### FR-08 진도 화면

전체 120회 중 완료 수, 모듈별 완료 수, 총 학습일, 현재 streak, 복습 예정 개념 수를 간단히 보여준다.

### FR-09 승인 콘텐츠 제한

일반 학습 모드에서는 `APPROVED` 콘텐츠만 선택한다. 승인 콘텐츠가 없으면 조용히 잘못된 내용을 대신 표시하지 않고 명시적인 콘텐츠 부족 상태를 보여준다.

### FR-10 출처 확인

각 오늘의 학습 단위에서 출처 제목과 기관을 접어 보기 형태로 확인할 수 있다.

### FR-11 하루 한 단위 제한

오늘 단위를 완료한 뒤 다음 커리큘럼 단위를 자동 제공하지 않는다.

### FR-12 백업

Phase 7에서 현재 로컬 진도 전체를 JSON으로 내보내고, 스키마 검증 후 다시 가져올 수 있어야 한다.

## 3.4 비기능 요구사항

### NFR-01 단순성

읽기 쉬운 단일 Next.js 애플리케이션으로 유지한다. 마이크로서비스, 이벤트 버스, 벡터 DB, Kubernetes를 사용하지 않는다.

### NFR-02 성능

로컬 콘텐츠와 상태를 이용하는 주요 화면은 일반적인 데스크톱·모바일 브라우저에서 불필요한 대기 없이 열린다. 인위적인 로딩 애니메이션을 넣지 않는다.

### NFR-03 복원 가능성

세션의 각 주요 단계 완료 시 상태를 저장한다. 저장 실패 시 완료한 것처럼 표시하지 않는다.

### NFR-04 결정 가능성

같은 날짜에 새로고침해도 오늘 단위와 복습 문제 구성이 바뀌지 않는다. 선택 결과를 DailySession에 저장한다.

### NFR-05 콘텐츠 안전성

자동 구조 검증과 사람의 과학 검토를 분리한다. 자동 테스트 통과만으로 콘텐츠를 승인하지 않는다.

### NFR-06 접근성

키보드로 주요 흐름을 완료할 수 있고, 버튼·라벨·오류 메시지가 명확해야 한다.

### NFR-07 비밀정보

OpenAI API 키 등 비밀값을 클라이언트 번들, 저장소, localStorage에 넣지 않는다.

## 3.5 MVP 제외 범위

- 회원가입, 로그인, 소셜 로그인
- 다중 사용자와 사용자별 권한
- 결제, 구독, 관리자 포털
- 친구, 랭킹, 레벨, 배지, 캐릭터, 포인트, 상점
- 푸시 알림과 네이티브 앱
- 코딩 실습과 실행 환경
- 신약개발, 약리학, 임상시험 별도 과정
- 음성, 3D, AR/VR
- 고급 적응형 추천, SM-2, FSRS
- 서버 DB와 기기 간 동기화
- 자동 콘텐츠 생성·자동 승인

## 3.6 제품 성공 판단 기준

MVP는 다음을 만족하면 성공으로 본다.

1. 루트 접속 후 별도 과정 선택 없이 오늘 학습이 열린다.
2. 새로고침 후에도 현재 단계가 유지된다.
3. 미완료 커리큘럼 단위를 건너뛰지 않는다.
4. 일반적인 하루 세션이 실제 사용 기준 약 10~18분 범위에 들어온다.
5. 승인되지 않은 콘텐츠가 일반 학습 경로에 노출되지 않는다.
6. 복습 정답·오답에 따라 다음 복습일이 규칙대로 변경된다.
7. 2주 이상 실제 사용해도 데이터 손실이나 학습 막힘이 없다.

15분은 초 단위 강제 제한이 아니라 콘텐츠 분량과 문제 수를 제어하는 설계 목표다. 시간이 지났다고 세션을 자동 종료하지 않는다.

---

# 4. 핵심 사용자 흐름

## 4.1 정상적인 미완료일

1. 사용자가 `/`에 접속한다.
2. 앱이 `Asia/Seoul` 기준 오늘 날짜를 계산한다.
3. 오늘 미완료 DailySession이 있으면 그대로 복원한다.
4. 없으면 다음을 한 번만 결정해 저장한다.
   - 오늘의 커리큘럼 단위 1개
   - 오늘의 복습 문제 최대 3개(필요 시 최대 5개)
5. 일일 복습을 푼다.
6. 오늘의 학습 내용을 읽는다.
7. 오늘의 이해 확인 문제를 푼다.
8. 직접 설명을 입력한다.
9. 핵심 확인 항목을 보고 스스로 평가한다.
10. 완료 버튼을 누른다.
11. 단위 진도와 개념 복습 일정이 원자적으로 갱신된다.
12. 오늘 완료 화면을 본다.

## 4.2 복습 대상이 없는 날

- “오늘 예정된 이전 복습이 없습니다”를 짧게 보여주고 오늘의 학습으로 바로 이동한다.
- 빈 화면이나 가짜 복습 문제를 만들지 않는다.

## 4.3 통합 단위인 날

- 일일 복습은 동일하게 수행한다.
- “새 개념” 대신 “이번 모듈 연결하기” 같은 제목을 사용한다.
- 기존 개념 간 비교·흐름·관계를 읽는다.
- 통합 문제와 직접 설명을 수행한다.
- 새로운 ConceptProgress를 임의로 만들지 않는다.

## 4.4 오늘 이미 완료한 경우

- 완료 시각, 오늘 학습 제목, 간단한 결과를 보여준다.
- 다음 단위를 시작하는 버튼을 제공하지 않는다.
- `진도 보기`, `오늘 내용 다시 보기`만 제공할 수 있다.
- 다시 보기는 진도나 복습 일정을 중복 갱신하지 않는다.

## 4.5 승인 콘텐츠가 부족한 경우

- “다음 학습 콘텐츠가 아직 승인되지 않았습니다”라고 표시한다.
- DRAFT를 대신 노출하지 않는다.
- 진도 화면과 기존 완료 내용은 계속 열 수 있다.

## 4.6 저장 데이터가 손상된 경우

- 앱이 무한 로딩하거나 상태를 덮어쓰지 않는다.
- 복구 가능한 이전 버전이면 migration을 시도한다.
- 복구할 수 없으면 내보내기/초기화 안내를 제공한다.
- 초기화는 사용자 명시 동작 없이 자동 실행하지 않는다.

---

# 5. 화면 목록과 목적

## 5.1 오늘 학습 화면 `/`

제품의 핵심이자 기본 진입점이다.

한 화면 안에서 단계형 UI로 다음을 순서대로 보여준다.

- 복습
- 오늘의 학습
- 이해 확인 문제
- 직접 설명과 자기 확인
- 완료

필수 요소:

- 현재 단계
- 남은 단계의 간단한 표시
- 저장 상태 또는 저장 오류
- 오늘 학습 제목
- 진도 화면 링크

금지 요소:

- 대시보드 우선 노출
- 여러 학습 과정 선택
- 다음 단원 추천 카드
- 점수 경쟁과 게임화 장식

## 5.2 오늘 완료 상태 `/` 내부 상태

오늘 학습을 이미 완료했을 때 같은 경로에서 보여준다.

목적:

- 완료 사실 확인
- 중복 완료 방지
- 오늘 배운 내용 다시 보기
- 전체 진도로 이동

별도 URL은 만들지 않아도 된다.

## 5.3 진도 화면 `/progress`

목적:

- 전체 120회 중 현재 위치 확인
- 12개 모듈별 완료 수 확인
- 총 학습일과 streak 확인
- 오늘 기준 복습 예정/연체 개념 수 확인

상세 분석 그래프, 점수 추세, 랭킹은 넣지 않는다.

## 5.4 콘텐츠 부족/오류 상태

별도 라우트보다 오늘 학습 화면의 명시적 상태로 구현한다.

- 승인 콘텐츠 없음
- 콘텐츠 구조 오류
- 로컬 저장 실패
- AI 피드백 실패(Phase 6)

각 상태는 원인과 사용자가 할 수 있는 최소 행동만 안내한다.

## 5.5 별도 화면으로 만들지 않는 것

- 로그인
- 설정
- 관리자
- 콘텐츠 편집기
- 출처 전용 페이지
- AI 채팅
- 통계 분석 대시보드

Phase 7의 가져오기·내보내기는 `/progress` 하단의 작은 관리 영역으로 넣을 수 있다.

---

# 6. 최소 데이터 모델

아래 구조는 구현 방향을 고정하기 위한 논리 모델이다. 실제 타입명은 저장소 기존 규칙을 따르되 의미를 바꾸지 않는다.

```ts
type ApprovalStatus = "DRAFT" | "REVIEWED" | "APPROVED" | "RETIRED";

type CurriculumUnitType = "CONCEPT" | "INTEGRATION";

type QuestionType =
  | "MULTIPLE_CHOICE"
  | "TRUE_FALSE"
  | "FILL_BLANK"
  | "SHORT_ANSWER";

type MasteryState =
  | "NOT_STARTED"
  | "LEARNING"
  | "REVIEWING"
  | "MASTERED"
  | "WEAK";

type DailyStep =
  | "REVIEW"
  | "TODAY_UNIT"
  | "QUIZ"
  | "EXPLAIN"
  | "COMPLETE";
```

## 6.1 콘텐츠 모델

```ts
type CurriculumCatalog = {
  contentVersion: string;
  title: string;
  totalUnits: 120;
  totalConceptUnits: 96;
  totalIntegrationUnits: 24;
  moduleIds: string[];
};

type ModuleContent = {
  id: string;
  orderIndex: number;
  titleKo: string;
  titleEn: string;
  purpose: string;
  expectedConceptUnits: number;
  expectedIntegrationUnits: 2;
  unitIds: string[];
};

type SourceRecord = {
  id: string;
  title: string;
  organization: string;
  url: string;
  sourceType: "TEXTBOOK" | "GOVERNMENT" | "UNIVERSITY" | "REVIEW" | "PRIMARY";
  accessedAt: string;
  licenseNote?: string;
};

type SourceCitation = {
  sourceId: string;
  locator?: string;
  supports: string;
};

type BaseUnit = {
  id: string;
  moduleId: string;
  type: CurriculumUnitType;
  globalOrder: number;
  moduleOrder: number;
  status: ApprovalStatus;
  titleKo: string;
  titleEn: string;
  learningGoal: string;
  whyItMatters: string;
  estimatedMinutes: {
    priorReview: number;
    content: number;
    quiz: number;
    explanation: number;
    total: number;
  };
  questions: Question[];
  explanationPrompt: string;
  selfCheckKeyPoints: string[];
  sourceCitations: SourceCitation[];
};

type ConceptUnit = BaseUnit & {
  type: "CONCEPT";
  conceptId: string;
  oneSentenceDefinition: string;
  coreExplanation: string;
  connectionsToPrevious: string[];
  keyPoints: string[];
  commonMisconceptions: string[];
  simpleExample?: string;
  dataConnection?: string;
};

type IntegrationUnit = BaseUnit & {
  type: "INTEGRATION";
  coveredConceptIds: string[];
  integrationExplanation: string;
  comparisonPoints: string[];
};

type Question = {
  id: string;
  type: QuestionType;
  prompt: string;
  conceptIds: string[];
  purpose: "DEFINITION" | "FUNCTION" | "CONNECTION";
  options?: { id: string; text: string }[];
  correctOptionIds?: string[];
  acceptedAnswers?: string[];
  explanation: string;
};
```

규칙:

- 객관식 선택지는 최소 2개다.
- O/X는 내부적으로 두 선택지를 가진 객관식으로 처리해도 된다.
- 빈칸·단답형은 공백, 대소문자, 기본 문장부호 정규화 후 비교한다.
- 단답형은 복잡한 자연어 의미 판정을 하지 않는다.
- 직접 설명은 `Question`이 아니라 별도 reflection 데이터다.

## 6.2 학습 상태 모델

```ts
type UnitProgress = {
  unitId: string;
  status: "NOT_STARTED" | "IN_PROGRESS" | "COMPLETED";
  startedAt?: string;
  completedAt?: string;
};

type ConceptProgress = {
  conceptId: string;
  masteryState: MasteryState;
  intervalStage: 0 | 1 | 2 | 3 | 4 | 5;
  nextReviewOn?: string; // Asia/Seoul의 YYYY-MM-DD
  lastReviewedAt?: string;
  lastReviewCorrect?: boolean;
};

type AnswerRecord = {
  questionId: string;
  submittedAnswer: string | string[] | boolean;
  correct?: boolean;
  answeredAt: string;
};

type ExplanationRecord = {
  text: string;
  submittedAt: string;
  selfAssessment: "UNDERSTOOD" | "REVIEW_NEEDED";
  aiFeedback?: {
    status: "NOT_REQUESTED" | "SUCCEEDED" | "FAILED";
    summary?: string;
    missingKeyPoints?: string[];
    misconceptionWarning?: string;
  };
};

type DailySession = {
  id: string; // 예: 2026-07-11
  localDate: string;
  timezone: "Asia/Seoul";
  unitId: string;
  reviewQuestionIds: string[];
  currentStep: DailyStep;
  startedAt: string;
  completedAt?: string;
  reviewAnswers: AnswerRecord[];
  quizAnswers: AnswerRecord[];
  explanation?: ExplanationRecord;
};

type AppState = {
  schemaVersion: 1;
  contentVersion: string;
  unitProgress: Record<string, UnitProgress>;
  conceptProgress: Record<string, ConceptProgress>;
  sessionsByDate: Record<string, DailySession>;
};
```

## 6.3 저장 원칙

- localStorage 키는 하나의 버전 키로 관리한다. 예: `biology15.app-state.v1`
- streak, 총 학습일, 모듈별 완료 수는 저장하지 않고 완료 세션과 단위 진도에서 계산한다.
- 오늘의 복습 문제 ID와 단위 ID는 세션 생성 시 저장하여 새로고침 때 바뀌지 않게 한다.
- 콘텐츠 버전이 바뀌어도 기존 완료 ID를 보존한다.
- 삭제·이름 변경이 필요한 콘텐츠는 migration 없이 임의 변경하지 않는다.

## 6.4 복습 상태 전이

초기 단순 규칙:

1. 개념 단위 최초 완료
   - `LEARNING`
   - `intervalStage = 0`
   - 다음 날 복습

2. 복습 정답
   - stage 0 → 1: 3일 후
   - stage 1 → 2: 7일 후
   - stage 2 → 3: 14일 후
   - stage 3 → 4: 30일 후
   - stage 4 → 5: `MASTERED`, 정규 다음 복습 없음
   - WEAK 상태였다면 정답 시 `REVIEWING`으로 복귀

3. 복습 오답
   - `WEAK`
   - stage를 1단계 낮추되 0 미만으로 내리지 않음
   - 다음 날 다시 복습

4. 직접 설명에서 `REVIEW_NEEDED` 선택
   - MASTERED를 자동 해제하지 않음
   - 현재 개념이 MASTERED가 아니라면 다음 복습일을 다음 날보다 늦지 않게 당길 수 있음

---

# 7. 콘텐츠 파일 구조와 검증 방식

## 7.1 권장 디렉터리

```text
content/
  catalog.json
  sources.json
  review-ledger.json
  modules/
    module-01.json
    module-02.json
    ...
    module-12.json
  units/
    m01-c01.json
    m01-c02.json
    ...
    m01-r01.json
    m01-r02.json
    ...
    m12-r02.json

docs/
  product-requirements.md
  content-authoring-guide.md
  content-review-checklist.md
  phase-plan.md

src/
  lib/
    content/
      schema.ts
      loader.ts
      validate.ts
    learning/
      session.ts
      review-scheduler.ts
      progress.ts
    storage/
      app-state.ts
      migrations.ts

tests/
  fixtures/
    content/
  content/
  learning/
  storage/
```

파일명 규칙:

- 모듈: `module-01.json` ~ `module-12.json`
- 개념 단위: `m{module}-c{order}.json`
- 통합 단위: `m{module}-r01.json`, `m{module}-r02.json`
- ID와 파일명은 한 번 공개한 뒤 임의 변경하지 않는다.

## 7.2 자동 검증

`npm run content:validate`가 최소한 다음을 확인한다.

### 구조 검증

- 모든 JSON이 Zod 스키마를 통과함
- 필수 필드 존재
- enum 값 유효
- 질문 유형별 정답 필드 유효

### 참조 무결성

- moduleId, unitId, conceptId, sourceId 참조가 존재함
- 통합 단위의 coveredConceptIds가 앞에서 등장한 개념을 가리킴
- 문제의 conceptIds가 유효함

### 순서와 수량

- module order가 1~12로 중복 없이 이어짐
- globalOrder가 1~120으로 중복·누락 없이 이어짐
- 총 96 CONCEPT, 24 INTEGRATION
- 모듈별 예상 수량이 기준 문서와 일치함
- 각 모듈에 통합 단위 2개가 존재함

### 문제 검증

- question ID 전체 고유
- 객관식 정답이 실제 option을 가리킴
- 중복 option과 빈 정답 없음
- 문제 explanation 존재
- 단위마다 정의·기능·연결 유형이 가능한 범위에서 균형을 이룸

### 공개 검증

별도 명령 `npm run content:validate:approved`를 둔다.

- 학습 경로에 포함될 파일이 모두 APPROVED인지 확인
- 각 승인 단위에 최소 2개의 독립 출처 참조가 있는지 확인
- review-ledger에 승인자, 승인일, 체크리스트 버전이 있는지 확인

이 검증은 “출처가 두 개 적혀 있다”는 구조만 확인한다. 출처가 실제 주장을 뒷받침하는지는 사람이 검토한다.

## 7.3 사람의 과학 검토

각 단위는 다음 체크를 마쳐야 APPROVED로 바꿀 수 있다.

1. 핵심 주장마다 신뢰할 수 있는 근거가 있는가?
2. 독립된 자료 2개 이상에서 핵심 사실을 교차 확인했는가?
3. 최신 논쟁이나 예외를 입문자용 단정으로 왜곡하지 않았는가?
4. 한국어 용어와 영어 병기가 정확한가?
5. 비유가 실제 기전을 잘못 전달하지 않는가?
6. 문제의 정답과 해설이 본문·출처와 일치하는가?
7. 직접 설명의 self-check key point가 핵심 이해를 반영하는가?
8. 원문 문장을 과도하게 복제하지 않았는가?
9. 15분 세션에 맞는 분량인가?
10. 검토자와 날짜를 review-ledger에 기록했는가?

## 7.4 AI와 Codex의 역할 제한

Codex가 할 수 있는 것:

- 스키마와 검증 코드 작성
- DRAFT 파일의 구조 오류 수정
- 사용자가 제공한 승인 콘텐츠 import
- 출처 메타데이터 누락 보고
- 승인 현황 리포트 생성

Codex가 독자적으로 하면 안 되는 것:

- 과학적 정확성 검토 없이 APPROVED 전환
- 출처를 확인하지 않은 핵심 설명 작성
- 120개 콘텐츠를 한 번에 자동 생성하고 완성으로 간주
- 정답 기준을 임의 변경
- DRAFT를 앱에 자동 노출

---

# 8. Phase 0~7 개발 순서 재검토 결과

## Phase 0. 제품·콘텐츠 계약 확정

코드 없이 PRD, 사용자 흐름, 데이터 모델, 콘텐츠 스키마, 테스트 전략, Phase 경계를 확정한다.

## Phase 1. 하루 학습 흐름 수직 슬라이스

하드코딩된 개발용 fixture 하나로 `/`에서 복습 → 오늘 학습 → 객관식 → 직접 설명 → 완료 → 재접속 복원이 작동하게 한다.

직접 설명은 이미 이 단계에 포함한다. AI만 Phase 6으로 미룬다.

## Phase 2. 콘텐츠 파일·검증 기반

JSON 스키마, 로더, validator, 테스트 fixture를 구현한다. 실제 과학 콘텐츠를 자동 승인하지 않는다.

## Phase 3. 여러 날 진도·세션 관리

버전된 localStorage 상태, 오늘 세션 자동 선택·재개, 다음 미완료 단위, 진도 화면, streak를 구현한다. DB는 도입하지 않는다.

## Phase 4. 복습 엔진

단순 간격 반복, WEAK 우선순위, 결정적 문제 선택, 하루 문제 수 제한을 구현한다. SM-2와 FSRS는 제외한다.

## Phase 5. 120회 카탈로그·콘텐츠 투입 체계

원래 제안처럼 Codex가 120회 전체 설명을 한 번에 생성하게 하지 않는다.

Phase 5는 두 부분으로 운영한다.

- **5A 개발 작업:** 12개 모듈과 120개 unit shell, 승인 리포트, import/validation 체계 완성
- **5B 콘텐츠 작업:** 모듈별로 DRAFT 작성 → 사람 검토 → APPROVED 전환 → import를 반복

Phase 5B는 개발과 정확성 검토를 분리하기 위해 모듈 단위로 반복한다. Core MVP는 충분한 APPROVED 콘텐츠가 들어간 시점에 실제 학습 가능하다. 120회 전체 과정 완성 판정은 `content:validate:approved` 통과로 한다.

## Phase 6. 선택적 AI 피드백

AI는 직접 설명에 대한 보조 피드백만 제공한다. 키가 없거나 API가 실패해도 완료 가능하다. AI 결과는 숙달 상태를 바꾸지 않는다.

## Phase 7. 실제 사용 기반 개선

2~4주 사용 데이터와 오류를 바탕으로 분량·복습 규칙·UI 마찰을 조정한다. 사용 근거 없이 기능을 늘리지 않는다. 백업·가져오기와 필요한 안정화만 추가한다.

## 핵심 변경점

- DB 도입을 Phase 3에서 제거
- 직접 설명 UI를 Phase 1로 앞당김
- AI만 Phase 6에 유지
- Phase 5를 기술 작업과 콘텐츠 검토 작업으로 분리
- Phase 5 이전까지는 개발 fixture로 기능을 검증하고, 이를 승인 콘텐츠로 오인하지 않음

## 8.1 모듈별 커리큘럼 수량 검증

| # | 모듈 | CONCEPT | INTEGRATION | 합계 |
|---:|---|---:|---:|---:|
| 1 | 생명과학과 세포 | 8 | 2 | 10 |
| 2 | 생체분자와 에너지 | 7 | 2 | 9 |
| 3 | DNA·유전자·유전체 | 9 | 2 | 11 |
| 4 | RNA와 유전자 발현 | 8 | 2 | 10 |
| 5 | 세포분열·유전·진화의 기초 | 8 | 2 | 10 |
| 6 | 미생물과 바이러스 | 7 | 2 | 9 |
| 7 | 면역학 | 12 | 2 | 14 |
| 8 | 인체와 질병의 기초 | 7 | 2 | 9 |
| 9 | 실험과 측정의 초간단 개요 | 6 | 2 | 8 |
| 10 | 통계와 실험 설계의 초간단 개요 | 6 | 2 | 8 |
| 11 | 오믹스 핵심 개요 | 7 | 2 | 9 |
| 12 | 바이오 데이터와 분석 파이프라인 | 11 | 2 | 13 |
| **합계** | **12개 모듈** | **96** | **24** | **120** |

각 모듈은 INTEGRATION 단위 2개를 포함한다. 직접 합산 결과 `96 + (12 × 2) = 120`으로
문서에 선언된 총량과 일치한다.

## 8.2 핵심 요구사항 추적표

| 요구사항 | 구현 Phase | 비고 |
|---|---:|---|
| 오늘 학습 자동 시작·재개 | 1, 3 | 1은 단일 fixture, 3은 여러 날과 다음 미완료 단위 |
| 하루 한 단위 제한 | 1, 3 | 완료 후 다음 단위 자동 시작 금지 |
| 일일 복습 문제 | 1, 4 | 1은 수직 슬라이스, 4는 실제 일정·선정 엔진 |
| CONCEPT 또는 INTEGRATION 단위 | 2, 5 | 2는 구조, 5는 120개 카탈로그 |
| 이해 확인 문제 | 1, 2 | 1은 객관식, 2는 JSON 문제 유형과 채점 |
| 직접 설명 | 1 | AI 없이 자기 확인으로 완료 |
| 완료 저장 | 1, 3 | localStorage, 여러 날 AppState는 3 |
| 콘텐츠 JSON 로딩·스키마 검증 | 2 | 구조 검증은 과학적 승인과 별개 |
| 여러 날 진도·streak·총 학습일 | 3 | 완료 세션에서 통계 파생 |
| 복습 일정·취약 개념 | 4 | 단순 간격 반복과 WEAK 관리 |
| 전체 진도 화면 | 3 | `/progress` |
| 120개 카탈로그 | 5A | 본문 대량 생성이나 자동 승인 아님 |
| 콘텐츠 승인 상태 | 2, 5 | 일반 학습은 APPROVED만 노출 |
| 선택적 AI 피드백 | 6 | 완료·정답·숙달·승인을 통제하지 않음 |
| 백업과 복원 | 7 | 검증된 JSON 내보내기·가져오기 |
| Vercel 배포 | Phase 1 완료 후 | 연결은 사용자가 수행, 각 후속 Phase는 build 검증 |

---

# 9. Phase별 완료 조건과 검증 항목

## Phase 0 완료 조건

- PRD가 문서화됨
- 120회 수량과 모듈 순서가 고정됨
- Daily Review와 Integration Unit이 구분됨
- 사용자 흐름과 화면 목록이 확정됨
- localStorage 유지 결정이 기록됨
- 데이터 모델과 콘텐츠 파일 계약이 확정됨
- Phase별 범위·제외 항목·검증 계획이 문서화됨

검증:

- 문서 간 용어가 일치함
- 전체 수량 96 + 24 = 120
- 모든 필수 요구사항이 최소 한 Phase에 배정됨
- 제외 기능이 어느 Phase에도 섞이지 않음
- 코드 파일과 런타임 의존성 변경이 없음

## Phase 1 완료 조건

- `/`에서 개발 fixture 기반 오늘 세션이 자동 시작됨
- 복습, 오늘 학습, 객관식, 직접 설명, 완료 순서가 작동함
- 단계별 상태가 localStorage에 저장됨
- 새로고침 후 같은 단계와 입력이 복원됨
- 완료 후 같은 날 다음 단위를 자동 시작하지 않음
- 최소 단위·컴포넌트·E2E 테스트가 존재함

검증:

- lint
- typecheck
- unit/component tests
- production build
- 핵심 E2E: 첫 진입부터 완료, 새로고침 복원, 완료 재접속

## Phase 2 완료 조건

- JSON 콘텐츠 스키마가 discriminated union으로 구현됨
- loader가 APPROVED만 일반 모드에 반환함
- DRAFT/REVIEWED/RETIRED가 일반 학습에서 제외됨
- 구조·참조·순서 validator가 구현됨
- 테스트 fixture로 4개 문제 유형을 검증함
- production content를 Codex가 임의 APPROVED 처리하지 않음
- 저자 가이드와 검토 체크리스트가 있음

검증:

- `content:validate`
- 잘못된 fixture가 예상대로 실패하는 테스트
- 승인 상태 필터 테스트
- lint/typecheck/test/build

## Phase 3 완료 조건

- AppState가 버전된 단일 localStorage 키로 저장됨
- 오늘 미완료 세션 복원
- 오늘 세션 신규 생성 시 다음 미완료 승인 unit 선택
- 결석일에 단위를 건너뛰지 않음
- 완료 시 unit progress 갱신
- `/progress`에 전체·모듈별 진도, 총 학습일, streak 표시
- 통계가 저장 중복 없이 파생됨
- 저장 데이터 오류 처리와 최소 migration 경계가 있음

검증:

- 날짜 경계와 Asia/Seoul 테스트
- 미완료 재개 테스트
- 같은 날 중복 세션 방지
- 결석일 이후 다음 단위 테스트
- streak 계산 테스트
- lint/typecheck/test/build/E2E

## Phase 4 완료 조건

- 최초 완료 다음 날 복습 예약
- 정답 시 3/7/14/30일 규칙 적용
- 오답 시 WEAK 및 다음 날 재예약
- WEAK·연체·오늘 예정 순 우선순위
- 기본 3개, 최대 5개 제한
- 같은 daily session 안에서 질문 중복 없음
- 새로고침해도 선정 질문이 바뀌지 않음
- 오늘 전체 흐름이 한 단위로 유지됨

검증:

- 각 interval stage의 날짜 계산 테스트
- 오답 전이 테스트
- 우선순위 테스트
- 문제 수 상한 테스트
- 결정성 테스트
- lint/typecheck/test/build/E2E

## Phase 5 완료 조건

### 5A

- 12개 module metadata 존재
- 120개 unit ID·순서·유형 shell 존재
- 수량 validator 통과
- 승인 현황 리포트 생성 가능
- DRAFT는 일반 학습에서 제외
- 승인 콘텐츠 import 절차 문서화

### 5B 전체 과정 완료 판정

- 96개 개념 단위와 24개 통합 단위가 실제 내용 보유
- 각 단위에 문제, 해설, 설명 prompt, self-check 존재
- 핵심 출처 2개 이상 기록
- review-ledger 완료
- 전체 120개가 APPROVED
- `content:validate:approved` 통과

검증:

- 구조·참조·수량 검증
- 승인 게이트 검증
- 앱이 1번부터 120번까지 순서대로 선택 가능한 시뮬레이션 테스트
- 콘텐츠 정확성은 자동 테스트와 별도로 사람 검토 기록 확인
- lint/typecheck/test/build

## Phase 6 완료 조건

- 직접 설명 제출 후 선택적으로 AI 피드백 요청 가능
- API 키는 서버에서만 사용
- 승인 콘텐츠의 key point를 피드백 기준으로 사용
- AI 실패·timeout·미설정 시 학습 완료 가능
- AI가 정답, APPROVED, mastery를 변경하지 않음
- 요청·응답 로그에 비밀키가 포함되지 않음

검증:

- API 성공 mock
- API 실패·timeout mock
- 키 미설정 fallback
- 악의적 응답/빈 응답 처리
- 핵심 흐름이 AI 없이 통과하는 E2E
- lint/typecheck/test/build

## Phase 7 완료 조건

- 실제 사용 데이터가 있을 때만 분량·간격 변경 근거를 문서화
- 상태 JSON 내보내기
- 가져오기 전 schema/version 검증
- 잘못된 파일이 기존 상태를 덮어쓰지 않음
- 사용 중 발견된 막힘·오류·느린 화면 개선
- 필요성이 확인되지 않은 기능은 추가하지 않음
- 접근 제한은 사용자가 명시적으로 요구할 때만 구현

검증:

- export/import round-trip
- 잘못된 import 거부
- 이전 schema migration 또는 명시적 거부
- 핵심 E2E 회귀 테스트
- 실제 사용 전후 변경 근거 문서
- lint/typecheck/test/build

---

# 10. Codex 공통 실행 규칙

아래 모든 Phase 프롬프트는 다음을 전제로 한다.

- 저장소의 lockfile을 확인해 npm, pnpm, yarn 중 기존 패키지 매니저를 사용한다.
- 아래 예시는 npm 명령이다. 다른 lockfile이면 같은 의미의 명령으로 바꾼다.
- 저장소에 이미 있는 규칙·구조·테스트를 우선한다.
- Phase 범위를 벗어난 선행 구현을 하지 않는다.
- 테스트 실패를 숨기거나 삭제해 통과시키지 않는다.
- 작업 후 다음 Phase로 넘어가지 않는다.

권장 검증 명령:

```bash
npm run lint
npm run typecheck
npm run test:run
npm run build
npm run test:e2e
```

콘텐츠 Phase에서는 추가:

```bash
npm run content:validate
npm run content:validate:approved
```

실제 저장소의 script 이름이 다르면 먼저 확인하고, 동일 목적의 기존 명령을 사용한다. 필요한 script가 없고 현재 Phase 범위에 포함된다면 최소한으로 추가한다.

---

# 11. Codex용 Phase별 프롬프트 사본(비기준 참고용)

이 절은 초기 명세에 포함된 참고 사본이다. 실제 실행에는
[`codex-phase-prompts.md`](codex-phase-prompts.md)의 최신 프롬프트만 사용한다.

## Phase 0 프롬프트

```text
이 저장소에서 개인용 생명과학 15분 학습 웹서비스의 Phase 0만 진행해라.

[가장 먼저 할 일]
1. 현재 작업 디렉터리, Git 상태, 현재 브랜치, 최근 커밋을 확인해라.
2. 저장소의 파일 구조와 기존 문서, package.json, lockfile이 있으면 읽어라.
3. 이미 구현된 코드나 문서가 있다면 현재 상태를 10줄 이내로 요약해라.
4. 기존 변경사항을 덮어쓰거나 되돌리지 마라.

[프로젝트 고정 조건]
- 사용자 한 명만 사용하는 개인용 서비스다.
- 사이트 루트에 접속하면 오늘 학습이 즉시 시작되거나 이어져야 한다.
- 하루 흐름은 일일 복습 → 오늘의 커리큘럼 단위 → 이해 확인 문제 → 직접 설명 → 완료다.
- 총 120회이며 새 개념 96회, 복습·통합 단위 24회다.
- 생명과학 기초부터 면역학, 오믹스, 바이오 데이터까지 다룬다.
- 신약개발은 별도 과정으로 만들지 않는다.
- 실험·측정·통계는 최소 개념만 다룬다.
- 면역학은 핵심 정규 모듈이다.
- 코딩 실습은 넣지 않는다.
- 상용화, 다중 사용자, 로그인, 결제, 게임화는 범위 밖이다.
- 과학 콘텐츠의 구조 검증과 과학적 정확성 승인을 구분한다.
- AI나 Codex가 콘텐츠를 자동 APPROVED 처리하면 안 된다.

[이번 Phase 범위]
코드는 구현하지 말고 문서만 작성 또는 갱신해라.

다음 문서를 저장소의 기존 문서 구조에 맞춰 작성해라.
- 제품 요구사항 문서
- 핵심 사용자 흐름
- 화면 목록과 목적
- 최소 데이터 모델
- JSON 콘텐츠 파일 구조와 검증 규칙
- Phase 0~7 계획
- Phase별 완료 조건과 검증 항목
- 결정 기록: localStorage를 MVP 기본 저장소로 유지, JSON 콘텐츠 사용, AI 피드백은 선택적 Phase 6

반드시 다음 모호성을 문서에서 해소해라.
- 매일 수행하는 일일 복습과 120회 안의 24개 복습·통합 단위는 서로 다르다.
- 통합 단위인 날에는 새 개념 대신 연결·비교 학습을 제공한다.
- 결석한 날은 커리큘럼을 건너뛰지 않는다.
- 하루 완료 후 다음 단위를 자동 시작하지 않는다.
- 직접 설명은 Phase 1부터 존재하고, AI 없이 자기 확인으로 완료 가능하다.
- 프로덕션 학습에는 APPROVED 콘텐츠만 노출한다.

[제외]
- 애플리케이션 코드 생성 또는 수정
- 패키지 설치
- Next.js 초기화
- DB, 인증, API, AI 구현
- 과학 콘텐츠 대량 작성
- 다음 Phase 작업

[검증]
- 12개 모듈의 수량이 새 개념 96 + 통합 24 = 총 120인지 확인해라.
- 모든 필수 요구사항이 어느 Phase에서 구현되는지 추적 가능하게 확인해라.
- 제외 기능이 계획에 섞이지 않았는지 확인해라.
- Markdown 링크와 문서 내부 용어를 점검해라.
- 가능하면 git diff --check를 실행해라.
- 저장소에 Markdown 검사 명령이 이미 있으면 그것만 실행해라. 새 도구는 설치하지 마라.

[완료 보고]
작업이 끝나면 다음만 요약해라.
1. 확인한 저장소 상태
2. 생성·변경한 문서 목록
3. 확정한 주요 결정
4. 실행한 검증과 결과
5. 남은 리스크 또는 사용자가 검토할 항목

Phase 1은 진행하지 마라.
```

## Phase 1 프롬프트

```text
이 저장소에서 개인용 생명과학 15분 학습 웹서비스의 Phase 1만 구현해라.

[가장 먼저 할 일]
1. pwd, git status, 현재 브랜치, 최근 커밋을 확인해라.
2. 저장소 구조, README와 docs, package.json, lockfile, 기존 테스트 설정을 읽어라.
3. Phase 0 문서가 있으면 요구사항과 결정 사항을 먼저 요약해라.
4. 기존 미커밋 변경을 덮어쓰거나 되돌리지 마라.
5. 구현 전에 변경 계획을 짧게 제시한 뒤 작업해라.

[목표]
개발용 fixture 하나를 사용해 실제 하루 학습 흐름의 수직 슬라이스를 완성한다.

루트 경로 `/`에서 다음 순서가 동작해야 한다.
1. 일일 복습
2. 오늘의 학습
3. 객관식 이해 확인 문제
4. 직접 설명 입력
5. 핵심 항목을 보고 자기 확인
6. 완료

[필수 구현]
- 저장소가 비어 있다면 Next.js App Router + TypeScript strict 기반으로 최소 초기화한다.
- 기존 Next.js 프로젝트가 있다면 기존 버전과 구조를 유지한다.
- 스타일은 Tailwind가 이미 있으면 사용하고, 없으면 단순 CSS로 충분하다. UI 라이브러리를 추가하지 마라.
- 단일 개발용 fixture를 사용한다. 이 fixture는 실제 승인 과학 콘텐츠로 간주하지 않도록 코드와 이름에서 명확히 구분한다.
- 복습 문제와 오늘 문제는 Phase 1에서는 객관식만 지원한다.
- 직접 설명 textarea와 제출 동작을 구현한다.
- 제출 후 fixture의 self-check key point를 보여주고 `대체로 설명함`과 `다시 복습 필요` 중 하나를 선택하게 한다.
- 각 단계가 끝날 때 현재 단계와 답변을 localStorage에 저장한다.
- 새로고침하면 같은 날짜의 같은 세션과 마지막 단계가 복원되어야 한다.
- 오늘 완료 후에는 완료 화면을 보여주고 다음 단위를 자동 시작하지 않는다.
- 오늘 내용 다시 보기는 허용하되 진도를 다시 갱신하지 않는다.
- `/progress`는 이번 Phase 범위가 아니다. 완료 화면의 진도 링크는 비활성 또는 숨겨도 된다.
- 시간대는 Asia/Seoul 기준 날짜 유틸리티를 사용하되 과도한 날짜 라이브러리는 추가하지 마라.

[테스트]
- 학습 단계 상태 전이 단위 테스트
- localStorage 저장·복원 테스트
- 오늘 완료 후 중복 완료 방지 테스트
- 주요 컴포넌트 테스트
- 가능하면 Playwright로 다음 핵심 E2E를 추가한다.
  1. 첫 진입부터 완료까지
  2. 중간 새로고침 후 재개
  3. 완료 후 재접속 시 완료 상태 유지

[제외]
- 실제 120회 콘텐츠
- JSON 콘텐츠 로더와 전체 스키마
- 복습 간격 엔진
- 진도 대시보드
- DB, Prisma, Drizzle, Supabase
- 회원가입과 인증
- OpenAI API와 AI 피드백
- 게임화, 점수, 배지, 캐릭터
- 코딩 실습
- 다음 Phase 작업

[검증]
저장소의 package manager와 기존 script를 사용해 최소 다음 목적의 명령을 실행해라.
- lint
- typecheck
- non-watch test run
- production build
- 설정했다면 핵심 E2E

npm 기준 예시:
npm run lint
npm run typecheck
npm run test:run
npm run build
npm run test:e2e

실패가 있으면 원인을 숨기지 말고 현재 Phase 범위 안에서 수정한 뒤 다시 실행해라. 테스트를 삭제하거나 완화해서 통과시키지 마라.

[완료 보고]
1. 구현한 사용자 흐름
2. 생성·변경 파일
3. localStorage에 저장하는 값
4. 테스트와 검증 명령별 결과
5. 알려진 제한

Phase 2는 진행하지 마라.
```

## Phase 2 프롬프트

```text
이 저장소에서 개인용 생명과학 15분 학습 웹서비스의 Phase 2만 구현해라.

[가장 먼저 할 일]
1. Git 상태, 브랜치, 최근 커밋을 확인해라.
2. package manager, scripts, src 구조, Phase 0 문서, Phase 1 구현과 테스트를 읽어라.
3. Phase 1의 실제 동작과 기존 미커밋 변경을 요약해라.
4. 구현 전에 변경 계획을 짧게 제시해라.
5. 기존 변경을 임의로 되돌리지 마라.

[목표]
코드를 수정하지 않고 JSON 파일을 추가해 콘텐츠를 확장할 수 있는 기반을 만든다. 자동 구조 검증과 사람의 과학 검토를 명확히 분리한다.

[필수 구현]
- Zod 또는 저장소에 이미 있는 동급 검증 도구로 JSON 스키마를 구현한다.
- CurriculumCatalog, ModuleContent, SourceRecord, ConceptUnit, IntegrationUnit, Question을 검증한다.
- ConceptUnit과 IntegrationUnit은 discriminated union으로 구분한다.
- 승인 상태는 DRAFT, REVIEWED, APPROVED, RETIRED다.
- 일반 학습 loader는 APPROVED만 반환해야 한다.
- 테스트 또는 명시적 preview 경로에서만 DRAFT fixture를 읽을 수 있게 하되, 프로덕션 기본값에서 DRAFT를 노출하지 마라.
- 객관식, O/X, 빈칸, 짧은 단답형의 스키마와 최소 채점기를 구현한다.
- 직접 설명은 자동 채점 Question으로 만들지 마라.
- content/catalog.json, content/sources.json, content/modules, content/units 구조를 지원한다.
- 구조, 참조, ID 고유성, 순서, 질문 정답을 검사하는 `content:validate` script를 추가한다.
- `content:validate:approved` 명령의 기반을 만들되, 전체 120개가 아직 없어 실패하는 경우 그 이유가 명확한 report를 내게 한다.
- 테스트용 fixture로 Module 1 개념 단위 3개와 통합 단위 1개에 해당하는 최소 구조를 만든다. 이는 과학적으로 승인된 production content가 아니며 DRAFT 또는 test-only로 유지한다.
- Phase 1의 하드코딩 fixture 의존을 loader 인터페이스 뒤로 옮기되, 승인 콘텐츠 부재 때문에 핵심 개발 흐름이 깨지지 않도록 test/demo 경계를 명확히 유지한다.
- docs에 content-authoring-guide와 content-review-checklist를 작성한다.

[과학 콘텐츠 제한]
- Codex가 새로 작성한 설명이나 문제를 APPROVED로 표시하지 마라.
- 구조 검증 통과를 과학 정확성 검증으로 표현하지 마라.
- 출처를 확인하지 않은 사실을 채워 넣지 마라.
- 원문이나 문제를 대량 복제하지 마라.

[테스트]
- 정상 fixture validation
- 필수 필드 누락 실패
- 잘못된 source/concept/module 참조 실패
- 중복 ID와 잘못된 globalOrder 실패
- 문제 유형별 정상·오류 채점
- APPROVED 필터링
- DRAFT가 일반 loader에서 제외됨

[제외]
- 120개 전체 콘텐츠 작성
- 사람 대신 APPROVED 전환
- 여러 날 진도 로직
- 복습 스케줄러
- DB와 인증
- AI 기능
- 관리자 UI와 콘텐츠 편집기
- 다음 Phase 작업

[검증]
저장소 package manager로 다음 목적의 명령을 실행해라.
- content validation
- lint
- typecheck
- 전체 test
- production build
- 기존 핵심 E2E 회귀

npm 기준 예시:
npm run content:validate
npm run lint
npm run typecheck
npm run test:run
npm run build
npm run test:e2e

[완료 보고]
1. 콘텐츠 디렉터리와 스키마 요약
2. 자동 검증이 확인하는 것과 확인하지 못하는 것
3. 생성·변경 파일
4. DRAFT와 APPROVED 노출 규칙
5. 명령별 검증 결과

Phase 3은 진행하지 마라.
```

## Phase 3 프롬프트

```text
이 저장소에서 개인용 생명과학 15분 학습 웹서비스의 Phase 3만 구현해라.

[가장 먼저 할 일]
1. Git 상태, 브랜치, 최근 커밋을 확인해라.
2. Phase 0 문서와 Phase 1~2 코드, 콘텐츠 schema/loader, 테스트, package scripts를 읽어라.
3. 현재 학습 상태 저장 방식과 콘텐츠 승인 필터를 요약해라.
4. 미커밋 변경을 보존하고 구현 계획을 제시한 뒤 작업해라.

[목표]
여러 날 연속 사용할 수 있도록 오늘 세션, 커리큘럼 진도, 통계를 버전된 localStorage 상태로 관리한다.

[고정 결정]
- DB를 추가하지 않는다.
- 단일 localStorage key의 versioned AppState를 사용한다.
- 시간대는 Asia/Seoul이다.
- 결석한 날짜만큼 커리큘럼을 건너뛰지 않는다.
- 하루에 새 커리큘럼 단위는 하나만 완료할 수 있다.

[필수 구현]
- AppState schemaVersion과 contentVersion을 저장한다.
- unitProgress, conceptProgress, sessionsByDate를 관리한다.
- 오늘 미완료 DailySession이 있으면 그대로 복원한다.
- 오늘 세션이 없으면 가장 앞의 미완료 APPROVED unit 하나를 선택한다.
- 선택한 unitId와 reviewQuestionIds는 생성 시 저장해 새로고침 시 바뀌지 않게 한다. Phase 4 전에는 단순한 review fixture 또는 빈 목록이어도 된다.
- 완료 시 UnitProgress를 정확히 한 번만 갱신한다.
- ConceptUnit 최초 완료 시 ConceptProgress의 LEARNING 상태와 다음 날 복습 예정일을 설정할 수 있는 경계를 만든다. 상세 간격 전이는 Phase 4에서 완성한다.
- IntegrationUnit 완료는 새 concept를 만들지 않는다.
- `/progress` 화면을 추가한다.
- 전체 120회 중 완료 수, 모듈별 완료 수, 총 학습일, 현재 streak, 오늘 기준 복습 예정 수를 간단히 보여준다.
- streak와 총 학습일은 완료 세션에서 계산하고 중복 저장하지 않는다.
- 오늘 완료 후 다음 unit을 자동 열지 않는다.
- 승인된 다음 unit이 없으면 콘텐츠 부족 상태를 명확히 보여준다.
- localStorage parsing 실패나 schemaVersion 불일치 시 자동 초기화하지 말고 안전한 오류 상태 또는 지원 가능한 최소 migration을 구현한다.

[테스트]
- Asia/Seoul 날짜 키 생성
- 같은 날짜 DailySession 중복 방지
- 미완료 세션 복원
- 다음 미완료 APPROVED unit 선택
- 결석 후에도 다음 순번 유지
- IntegrationUnit이 concept progress를 만들지 않음
- 완료 idempotency
- 총 학습일과 streak 계산
- 손상된 저장 데이터 처리
- progress 화면 렌더링

[제외]
- 서버 DB, ORM, 로그인, 동기화
- 고급 복습 알고리즘
- AI 피드백
- 전체 120회 콘텐츠 작성
- 게임화와 분석 그래프
- import/export UI
- 다음 Phase 작업

[검증]
다음 목적의 명령을 실행해라.
- content validation
- lint
- typecheck
- test
- build
- 핵심 E2E: 첫날 완료, 다음 날짜 시뮬레이션, 미완료 재개, progress 반영

npm 기준 예시:
npm run content:validate
npm run lint
npm run typecheck
npm run test:run
npm run build
npm run test:e2e

[완료 보고]
1. AppState와 localStorage versioning
2. 오늘 unit 선택 규칙
3. 진도·streak 계산 방식
4. 변경 파일
5. 검증 결과와 남은 제한

Phase 4는 진행하지 마라.
```

## Phase 4 프롬프트

```text
이 저장소에서 개인용 생명과학 15분 학습 웹서비스의 Phase 4만 구현해라.

[가장 먼저 할 일]
1. Git 상태, 브랜치, 최근 커밋을 확인해라.
2. Phase 0 문서, 콘텐츠 모델, AppState, 날짜 유틸리티, DailySession 생성 코드와 테스트를 읽어라.
3. 현재 concept progress와 복습 관련 미완성 경계를 요약해라.
4. 기존 미커밋 변경을 보존하고 구현 계획을 제시한 뒤 작업해라.

[목표]
단순하고 검증 가능한 간격 반복 복습 엔진을 구현하되 하루 약 15분 흐름을 해치지 않는다.

[복습 규칙]
- 개념 최초 학습 완료 다음 날 첫 복습
- stage 0 정답: 3일 후
- stage 1 정답: 7일 후
- stage 2 정답: 14일 후
- stage 3 정답: 30일 후
- stage 4 정답: stage 5, MASTERED, 정규 다음 복습 없음
- 오답: WEAK, stage를 1 낮추되 0 미만 금지, 다음 날
- WEAK 상태에서 정답이면 REVIEWING으로 복귀
- 직접 설명의 REVIEW_NEEDED는 MASTERED를 자동 해제하지 않으며, 비MASTERED 개념의 복습일을 다음 날보다 늦지 않게 당길 수 있다.

[복습 대상 우선순위]
1. WEAK이면서 연체
2. WEAK이면서 오늘 예정
3. 일반 연체
4. 오늘 예정

같은 우선순위에서는 오래 연체된 순, 그 다음 conceptId의 안정적인 순서로 결정해라.

[문제 선택]
- 기본 3문제
- 최대 5문제
- 복습 가능한 개념이 3개보다 적으면 있는 만큼만 제공
- 같은 DailySession 안에서 questionId 중복 금지
- 가능한 경우 서로 다른 concept를 먼저 선택
- 같은 날짜에 새로고침해도 선택 결과가 바뀌지 않아야 하므로 선정 결과를 DailySession에 저장
- 문제 풀이 결과는 concept별로 집계해 복습 상태를 한 번만 갱신

[필수 구현]
- 순수 함수 중심의 review scheduler
- 날짜 계산과 상태 전이 분리
- review selection과 question selection 분리
- DailySession 생성 과정에 scheduler 연결
- review 단계 UI가 실제 선정 문제를 사용
- 정답/오답 제출 후 다음 복습일 갱신
- 완료 전후 중복 갱신 방지
- 15분을 넘긴다고 강제 종료하지 말고 문제 수 상한으로 제어

[테스트]
- 각 stage의 정답 다음 날짜
- 각 stage의 오답 전이
- WEAK 우선순위
- 연체 순서
- 3개 기본/5개 최대
- 부족한 대상 처리
- session 내 중복 방지
- deterministic selection
- 새로고침·중복 완료 시 일정이 두 번 이동하지 않음
- MASTERED 처리
- IntegrationUnit 자체가 review concept를 새로 만들지 않음

[제외]
- SM-2, FSRS, 머신러닝 추천
- 사용 시간에 따른 실시간 문제 추가
- DB와 서버 스케줄러
- 알림
- AI 피드백
- 전체 콘텐츠 생성
- 다음 Phase 작업

[검증]
다음 목적의 명령을 실행해라.
- content validation
- lint
- typecheck
- unit/component test
- build
- E2E: 복습 예정 concept가 있는 날의 전체 완료 흐름

npm 기준 예시:
npm run content:validate
npm run lint
npm run typecheck
npm run test:run
npm run build
npm run test:e2e

[완료 보고]
1. 복습 상태 전이 표
2. 우선순위와 문제 수 제한
3. 변경 파일
4. 테스트 케이스와 결과
5. 의도적으로 제외한 고급 기능

Phase 5는 진행하지 마라.
```

## Phase 5 프롬프트

```text
이 저장소에서 개인용 생명과학 15분 학습 웹서비스의 Phase 5A만 진행해라. Phase 5B의 과학 콘텐츠 대량 작성이나 승인은 하지 마라.

[가장 먼저 할 일]
1. Git 상태, 브랜치, 최근 커밋을 확인해라.
2. Phase 0 문서, curriculum 기준 문서, 콘텐츠 schema/validator/loader, 학습 흐름과 복습 엔진을 읽어라.
3. 현재 APPROVED/DRAFT 콘텐츠 수와 validation 결과를 확인해라.
4. 기존 미커밋 변경을 보존하고 변경 계획을 제시한 뒤 작업해라.

[목표]
12개 모듈과 120개 커리큘럼 단위의 카탈로그·ID·순서·유형을 정확히 고정하고, 사람이 검토한 콘텐츠를 모듈별로 안전하게 투입할 수 있는 체계를 완성한다.

[고정 커리큘럼 수량]
- 총 12개 모듈
- CONCEPT 96개
- INTEGRATION 24개
- 각 모듈 INTEGRATION 2개
- 총 120개 globalOrder가 1부터 120까지 연속

모듈별 CONCEPT 수:
1. 생명과학과 세포 8
2. 생체분자와 에너지 7
3. DNA·유전자·유전체 9
4. RNA와 유전자 발현 8
5. 세포분열·유전·진화의 기초 8
6. 미생물과 바이러스 7
7. 면역학 12
8. 인체와 질병의 기초 7
9. 실험과 측정의 초간단 개요 6
10. 통계와 실험 설계의 초간단 개요 6
11. 오믹스 핵심 개요 7
12. 바이오 데이터와 분석 파이프라인 11

[필수 구현]
- 기준 문서의 12개 module metadata를 JSON으로 등록한다.
- 120개 unit의 ID, moduleId, type, globalOrder, moduleOrder, title을 shell 형태로 정확히 등록한다.
- 과학 설명, 문제, 정답, 출처가 사람 검토되지 않았다면 shell은 DRAFT로 유지한다.
- DRAFT shell이 일반 학습 loader에 노출되지 않게 유지한다.
- 승인 현황을 module별로 출력하는 script 또는 report를 추가한다.
  - 전체 unit 수
  - DRAFT/REVIEWED/APPROVED/RETIRED 수
  - 누락된 필수 필드
  - 출처 수 부족
  - review-ledger 누락
- 사용자가 제공한 별도 승인 콘텐츠 파일이 저장소에 이미 있다면 schema 검증 후 import할 수 있다. 승인 근거가 없으면 상태를 바꾸지 마라.
- content-authoring-guide에 모듈 단위 Phase 5B 절차를 명시한다.
  1. DRAFT 작성
  2. 출처 교차 확인
  3. 문제·해설 검토
  4. REVIEWED
  5. 사람의 최종 승인과 ledger 기록
  6. APPROVED
- 앱이 승인된 unit만 순서대로 1~120까지 선택할 수 있는 simulation test를 추가한다. 승인되지 않은 중간 unit이 있으면 뒤 unit을 건너뛰지 않고 콘텐츠 부족 상태가 되어야 한다.
- `content:validate:approved`는 120개 전체가 승인되기 전에는 명확한 실패 report를 내고, 전체 승인 후에만 성공하게 한다.

[중요한 콘텐츠 제한]
- 120개 본문과 문제를 이번 작업에서 한 번에 자동 생성하지 마라.
- 검색·검토하지 않은 과학 설명을 채워 넣지 마라.
- Codex가 APPROVED를 결정하지 마라.
- 기준 문서의 제목 목록을 과학 콘텐츠 승인으로 해석하지 마라.
- 사용자 제공 콘텐츠에 구조 오류가 있으면 원문 의미를 임의 변경하지 말고 오류를 보고해라.

[테스트]
- 모듈별 수량
- 총 96/24/120 수량
- globalOrder 연속성
- 모든 shell 참조 무결성
- 승인 현황 report
- 중간 승인 누락 시 뒤 단위 건너뛰지 않음
- 전체 승인 simulation fixture에서 1~120 선택 순서

[제외]
- Phase 5B의 대량 과학 콘텐츠 작성·검수·승인
- 관리자 CMS
- DB
- AI 피드백
- 게임화
- 다음 Phase 작업

[검증]
다음 목적의 명령을 실행해라.
- content validation
- 승인 현황 report
- lint
- typecheck
- test
- build
- 기존 E2E 회귀

npm 기준 예시:
npm run content:validate
npm run content:report
npm run lint
npm run typecheck
npm run test:run
npm run build
npm run test:e2e

`content:validate:approved`는 현재 전체 콘텐츠가 승인되지 않았다면 실패가 정상이다. 이 경우 실패 내용을 완료 보고에 명확히 적고, 이를 코드 실패처럼 숨기지 마라.

[완료 보고]
1. 12개 모듈과 120개 shell 수량
2. 상태별 콘텐츠 수
3. APPROVED 전환을 하지 않은 이유
4. 생성·변경 파일
5. 검증 명령별 결과
6. Phase 5B에서 사람이 제공해야 할 콘텐츠 목록

Phase 5B나 Phase 6은 진행하지 마라.
```

## Phase 6 프롬프트

```text
이 저장소에서 개인용 생명과학 15분 학습 웹서비스의 Phase 6만 구현해라.

[가장 먼저 할 일]
1. Git 상태, 브랜치, 최근 커밋을 확인해라.
2. Phase 0 문서, 직접 설명 흐름, APPROVED 콘텐츠 loader, AppState, 테스트와 환경변수 규칙을 읽어라.
3. 현재 서비스가 AI 없이 완전히 완료 가능한지 먼저 확인해라.
4. 기존 미커밋 변경을 보존하고 구현 계획을 제시한 뒤 작업해라.

[선행 조건]
AI 없이 복습 → 오늘 학습 → 문제 → 직접 설명 → 완료가 작동해야 한다. 이 조건이 깨져 있다면 Phase 6 기능을 확장하지 말고, Phase 6 범위 안에서 회귀 원인만 최소 수정한 뒤 보고해라.

[목표]
직접 설명에 대해 선택적인 AI 보조 피드백을 제공한다. AI는 학습 완료, 정답 판정, mastery, 콘텐츠 승인을 통제하지 않는다.

[필수 구현]
- OpenAI 호출은 서버 측 route 또는 server action에서만 수행한다.
- API 키를 클라이언트 코드, localStorage, 로그, 저장소에 넣지 않는다.
- 키 미설정 시 AI 버튼을 숨기거나 “설정되지 않음” 상태로 처리하고, 기존 자기 확인으로 완료 가능하게 한다.
- 입력은 현재 APPROVED unit의 제목, learning goal, self-check key point, 사용자 설명으로 제한한다.
- 모델에게 다음 형식의 짧은 구조화 응답만 요청한다.
  - 잘 설명한 핵심
  - 빠진 핵심
  - 오개념 가능성 경고
  - 더 쉬운 한 문단 설명
- AI 응답은 조언이며 과학적 최종 판정이 아니라는 UI 문구를 둔다.
- AI 결과가 masteryState, 정답 기록, unit completion, approval status를 변경하지 못하게 한다.
- timeout, 네트워크 오류, 비정상 JSON, 빈 응답을 안전하게 처리한다.
- 실패해도 사용자의 설명과 기존 학습 진행을 잃지 않는다.
- 같은 설명에 대한 불필요한 반복 호출을 막는 최소 UI 상태를 둔다. 복잡한 캐시나 DB는 추가하지 마라.
- 필요한 최소 로그만 남기고 사용자 본문의 장기 서버 저장은 하지 않는다.

[콘텐츠 정확성 경계]
- AI는 APPROVED 콘텐츠의 key point를 기준으로 피드백한다.
- AI가 새로운 핵심 과학 사실을 만들어 승인 콘텐츠에 저장하지 않는다.
- AI 응답을 자동으로 문제 정답이나 콘텐츠 파일에 반영하지 않는다.

[테스트]
- 정상 구조화 응답 mock
- API 키 미설정
- timeout과 네트워크 실패
- 잘못된 JSON과 빈 응답
- 사용자가 매우 짧거나 긴 설명을 입력한 경우의 제한
- AI 실패 후에도 완료 가능
- AI 성공 후에도 mastery가 변경되지 않음
- 클라이언트 번들에 API 키가 포함되지 않음

[제외]
- AI 채팅 화면
- 벡터 DB, RAG, 임베딩
- AI 자동 채점과 숙달 결정
- AI 콘텐츠 자동 생성·승인
- 서버 DB와 장기 로그 분석
- 음성 입력
- 다음 Phase 작업

[검증]
다음 목적의 명령을 실행해라.
- content validation
- lint
- typecheck
- unit/integration test
- build
- E2E: AI 비활성 상태의 전체 흐름, AI 성공 mock, AI 실패 fallback

npm 기준 예시:
npm run content:validate
npm run lint
npm run typecheck
npm run test:run
npm run build
npm run test:e2e

[완료 보고]
1. AI가 받는 입력과 반환 형식
2. AI가 영향을 줄 수 없는 상태
3. 키·오류 처리 방식
4. 생성·변경 파일
5. 검증 결과

Phase 7은 진행하지 마라.
```

## Phase 7 프롬프트

```text
이 저장소에서 개인용 생명과학 15분 학습 웹서비스의 Phase 7만 진행해라.

[가장 먼저 할 일]
1. Git 상태, 브랜치, 최근 커밋을 확인해라.
2. Phase 0 문서, 현재 구현, 이슈 목록, 사용 기록 또는 사용자가 제공한 export 파일을 확인해라.
3. 실제 2~4주 사용 근거가 있는 개선 요청과 단순 아이디어를 구분해라.
4. 기존 미커밋 변경을 보존하고, 발견한 근거와 이번 변경 계획을 먼저 요약해라.

[목표]
새 기능을 늘리는 것이 아니라 실제 사용 중 발생한 학습 방해 요소, 데이터 손실 위험, 오류와 마찰을 줄인다.

[중요 원칙]
- 실제 사용 데이터가 없으면 학습 분량, 난이도, 복습 간격이 잘못됐다고 추측해 임의 변경하지 마라.
- 사용 근거가 없는 게임화, 추천, 대시보드, 관리자 기능을 추가하지 마라.
- 접근 제한은 사용자가 명시적으로 요구했거나 실제 배포 노출 문제가 확인된 경우에만 구현한다.

[필수 구현]
- `/progress`의 작은 관리 영역에서 AppState JSON 내보내기를 제공한다.
- JSON 가져오기 전에 schemaVersion, contentVersion 호환성, 데이터 구조를 검증한다.
- 잘못된 import는 기존 상태를 절대 덮어쓰지 않는다.
- import 전 현재 상태 백업 또는 명시적 확인 절차를 둔다.
- export → reset된 테스트 상태 → import의 round-trip이 가능해야 한다.
- localStorage quota, parsing 실패, 저장 실패 메시지를 개선한다.
- 실제 이슈/사용 기록이 있다면 각 개선에 근거를 연결해 문서화한다.
- 세션 시간이 반복적으로 18분을 넘는다는 근거가 있을 때만 문제 수나 콘텐츠 표시를 조정한다.
- 복습 과부하 근거가 있을 때만 우선순위 또는 최대 문제 수를 조정한다. 기존 단순성은 유지한다.
- 느린 렌더링, 키보드 접근, 모바일 가독성 등 확인된 문제를 최소 수정한다.
- 필요한 schema migration을 명시적으로 구현하고 테스트한다.

[사용 데이터가 없을 때]
- export/import와 안정성 개선까지만 수행한다.
- 학습량·난이도·복습 간격 튜닝은 하지 않았다고 보고한다.
- 가상의 사용자 행동이나 가짜 지표를 만들지 않는다.

[테스트]
- export/import round-trip
- 잘못된 JSON 거부
- 지원하지 않는 schemaVersion 거부 또는 migration
- import 실패 시 기존 상태 보존
- 완료 세션과 concept progress 보존
- contentVersion 차이 처리
- 주요 학습 E2E 전체 회귀
- 실제 수정한 오류의 회귀 테스트

[제외]
- 상용화 준비
- 다중 사용자와 클라우드 동기화
- 결제, 로그인, 관리자
- 게임화
- 새 커리큘럼 또는 코딩 실습
- 근거 없는 복습 알고리즘 교체
- 다음 프로젝트 Phase 자동 제안·구현

[검증]
다음 목적의 명령을 실행해라.
- content validation
- lint
- typecheck
- test
- build
- 전체 핵심 E2E

npm 기준 예시:
npm run content:validate
npm run lint
npm run typecheck
npm run test:run
npm run build
npm run test:e2e

[완료 보고]
1. 사용 근거가 있었던 개선과 없었던 항목
2. export/import와 데이터 보호 방식
3. 생성·변경 파일
4. 검증 명령별 결과
5. 임의로 추가하지 않은 기능

이 Phase 이후에도 별도 승인 없이 새로운 기능 확장을 진행하지 마라.
```

---

# 12. Phase 5B 콘텐츠 작업 운영 원칙

Phase 5B는 개발 Phase와 별개로 다음 루프를 모듈마다 반복한다.

1. 해당 모듈의 학습 목표와 단위 목록 확정
2. 신뢰 자료 조사
3. 각 단위 DRAFT 작성
4. 문제와 해설 별도 작성
5. 자동 `content:validate` 실행
6. 사람이 과학 정확성·난이도·저작권·15분 분량 검토
7. 수정 후 REVIEWED
8. 최종 확인 후 APPROVED와 ledger 기록
9. 앱에서 실제 하루 세션으로 시험
10. 다음 모듈로 이동

한 번에 120개를 승인하지 않는다. 권장 투입 순서는 Module 1을 먼저 완성해 실제 사용을 시작하고, 학습 진행 속도보다 앞서도록 다음 모듈을 순차 승인하는 방식이다.

과학 콘텐츠 조사·작성용 프롬프트는 Codex 개발 프롬프트와 분리해서 관리한다. 웹 검색과 출처 확인이 필요한 작업은 별도 연구 세션에서 수행해야 한다.

---

# 13. 최종 권장 실행 순서

1. 이 문서를 저장소의 Phase 0 기준으로 커밋
2. Phase 1 Codex 프롬프트 실행
3. 브라우저에서 실제 15분 흐름을 직접 한 번 완료
4. Phase 2 실행
5. Phase 3 실행
6. Phase 4 실행
7. Module 1 콘텐츠를 별도 조사·검토하여 APPROVED로 준비
8. Phase 5A 실행 후 Module 1 승인 콘텐츠 import
9. 실제 학습을 시작하면서 Modules 2~12를 순차 제작·승인
10. AI 피드백이 정말 필요할 때만 Phase 6 실행
11. 2~4주 실제 사용 기록이 생긴 뒤 Phase 7 실행

가장 중요한 출시 기준은 “기능이 많다”가 아니라 다음 세 가지다.

- 오늘 학습이 즉시 열린다.
- 15분 안팎으로 끝난다.
- 승인된 내용을 꾸준히 이어서 배울 수 있다.
