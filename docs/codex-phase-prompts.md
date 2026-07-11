# bioStudy Codex Phase 0~7 실행 프롬프트 v2

## 모든 Phase의 고정 실행 환경

- GitHub 저장소: `https://github.com/wanted1213/bioStudy.git`
- 기본 브랜치: `main`
- 배포 플랫폼: Vercel
- 기준 문서: 저장소의 `CODEX_MASTER.md` 또는 `docs/product-spec.md`
- 패키지 매니저는 기존 lockfile을 따른다. lockfile이 없으면 npm을 사용한다.
- Phase 1부터 production build 성공을 필수 완료 조건으로 한다.
- 실제 Vercel 프로젝트 연결과 Git push는 사용자가 수행할 수 있으므로, Codex는 인증정보를 요구하거나 임의로 원격 설정을 변경하지 않는다.
- Codex는 `git remote`, 사용자 인증, Vercel 계정 설정을 임의 변경하지 않는다.
- 커밋과 push는 사용자가 명시적으로 요청한 경우에만 수행한다.

## 권장 저장소 문서 배치

- `CODEX_MASTER.md`: 프로젝트 전체 고정 조건과 제품 요구사항
- `docs/phase-plan.md`: Phase 0~7 순서와 완료 조건
- `docs/content-authoring-guide.md`: 콘텐츠 작성 규칙
- `docs/content-review-checklist.md`: 과학 콘텐츠 사람 검수 기준

---

# 11. Codex용 Phase별 완성 프롬프트

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
- Vercel 배포를 전제로 SSR/build 단계에서 `window` 또는 `localStorage`를 직접 참조해 실패하지 않도록 클라이언트 경계를 명확히 한다.
- README에 로컬 실행, 검증, Vercel GitHub 연동 배포 절차를 짧게 기록한다. 실제 Vercel 계정 연결이나 배포 인증은 임의로 수행하지 마라.

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
6. Vercel 배포 준비 상태와 사용자가 수동으로 해야 하는 작업

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
- 사용자가 제공한 Vercel URL이 있으면 production smoke test: 루트 진입, 새로고침, 완료 상태 유지, `/progress` 접근

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
