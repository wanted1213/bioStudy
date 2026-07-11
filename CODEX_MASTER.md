# CODEX_MASTER.md — bioStudy

이 문서는 Codex가 매 Phase 시작 시 가장 먼저 읽는 공통 규칙이다. 세부 제품 요구사항은
[`docs/product-spec.md`](docs/product-spec.md), Phase별 실행 지시는
[`docs/codex-phase-prompts.md`](docs/codex-phase-prompts.md), 결정의 배경은
[`docs/decisions.md`](docs/decisions.md)를 따른다. 충돌 시 공통 제약은 이 문서, 제품 세부사항은
제품 명세, 해당 Phase의 작업 범위는 Phase 프롬프트 순으로 해석하고 충돌을 임의로 숨기지 않는다.

## 1. 프로젝트 목적

사용자가 웹사이트에 접속하면 별도의 선택 없이 오늘의 생명과학 학습을 시작하거나 이어가고, 약 15분 동안 `일일 복습 → 오늘의 학습 → 이해 확인 → 직접 설명 → 완료`를 수행하는 개인용 단일 사용자 웹서비스다.

## 2. 저장소와 배포

- GitHub: `https://github.com/wanted1213/bioStudy.git`
- 기본 브랜치: `main`
- 배포: Phase 1 구현 완료 후 Vercel에 연결하며, 이후 GitHub 연동 자동 배포
- 시간대: `Asia/Seoul`
- Phase 1부터 모든 Phase에서 production build 성공 필수

## 3. 절대 제약

- 상용 서비스로 확장하지 않는다.
- 다중 사용자, 회원가입, 로그인, 결제, 커뮤니티를 추가하지 않는다.
- 게임화, 배지, 캐릭터, 랭킹을 임의로 추가하지 않는다.
- 초기 120회 과정에 코딩 실습을 추가하지 않는다.
- DB, ORM, 마이크로서비스, Kubernetes, 벡터 DB를 추가하지 않는다.
- MVP 상태 저장은 localStorage를 사용한다.
- 개발보다 실제 15분 학습 지속성을 우선한다.
- AI/Codex가 과학 콘텐츠를 자동 APPROVED 처리하지 않는다.
- 과학적 정확성 검토와 코드 구조 검증을 구분한다.

## 4. 제품 핵심

- 사용자 한 명
- 총 120회: CONCEPT 96회 + INTEGRATION 24회
- 루트 `/` 접속 시 오늘 세션 자동 생성 또는 재개
- 하루 한 커리큘럼 단위만 완료
- 결석해도 커리큘럼을 건너뛰지 않음
- 오늘 완료 후 다음 단위를 자동 시작하지 않음
- 프로덕션 학습에는 APPROVED 콘텐츠만 노출
- 직접 설명은 Phase 1부터 AI 없이 완료 가능
- AI 피드백은 선택적 Phase 6

## 5. 기술 기본값

- Next.js App Router
- TypeScript strict
- 단순 CSS 또는 기존 Tailwind
- 콘텐츠 JSON + Zod 검증
- 진행 데이터 localStorage
- Vitest + React Testing Library
- 핵심 E2E Playwright
- Vercel 배포
- lockfile이 없으면 npm

## 6. 콘텐츠 범위

생명과학 기초, 생체분자, DNA·유전자·유전체, RNA와 유전자 발현, 세포분열·유전·진화, 미생물·바이러스, 면역학, 인체·질병, 실험·측정 최소 개요, 통계·실험 설계 최소 개요, 오믹스, 바이오 데이터·분석 파이프라인을 포함한다.

신약개발 별도 과정, 약리학, 임상시험, 분자 도킹, 실험 장비 조작, 통계 계산 훈련은 제외한다.

## 7. Phase 순서

- Phase 0: 제품·콘텐츠·개발 계약 문서 고정, 코드 없음
- Phase 1: 하루 학습 수직 슬라이스 + localStorage + Vercel 배포 가능 상태
- Phase 2: JSON 콘텐츠 스키마·로더·검증
- Phase 3: 여러 날 세션·진도·`/progress`
- Phase 4: 간격 반복 복습 엔진
- Phase 5A: 12개 모듈·120개 DRAFT shell·승인 리포트(자동 승인 없음)
- Phase 5B: 사람 검수 기반 실제 콘텐츠 투입(모듈 단위 별도 작업)
- Phase 6: 선택적 AI 직접 설명 피드백
- Phase 7: 실제 사용 기반 개선·백업·가져오기

## 8. Codex 공통 작업 규칙

매 Phase마다 다음을 지킨다.

1. 먼저 `pwd`, `git status`, 현재 브랜치, 최근 커밋, 저장소 구조를 확인한다.
2. 기존 문서와 이전 Phase 구현·테스트를 읽는다.
3. 미커밋 변경을 임의로 되돌리지 않는다.
4. 구현 전에 이번 Phase 변경 계획을 짧게 제시한다.
5. 이번 Phase 범위만 수행하고 다음 Phase를 시작하지 않는다.
6. 과도한 아키텍처와 의존성을 추가하지 않는다.
7. lint, typecheck, test, build를 실행한다.
8. 검증 실패를 숨기거나 테스트를 삭제해 통과시키지 않는다.
9. 완료 후 변경 파일, 구현 내용, 명령별 검증 결과, 남은 제한을 요약한다.
10. 원격 Git 설정, 인증, push, Vercel 계정 연결은 사용자 요청 없이 변경하지 않는다.

## 9. 세부 기준 문서

세부 요구사항, 데이터 모델, 화면, 콘텐츠 스키마, 완료 조건은 `docs/product-spec.md`를 기준으로 한다. Phase별 실행문은 `docs/codex-phase-prompts.md`를 기준으로 한다.
