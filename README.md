# bioStudy

`bioStudy`는 사이트를 열면 그날의 생명과학 학습을 바로 시작하거나 이어가는 개인용 단일 사용자 웹서비스다.

하루 약 15분 동안 `일일 복습 → 오늘의 커리큘럼 단위 → 이해 확인 문제 → 직접 설명 → 완료`를
진행한다. 전체 과정은 12개 모듈, 120회(CONCEPT 96회 + INTEGRATION 24회)로 구성한다.

현재는 **Phase 0 — 제품·콘텐츠 계약과 문서 확정** 단계이며, 애플리케이션 코드는 아직 없다.

## Phase 개요

- Phase 0: 제품·콘텐츠 계약과 문서 확정
- Phase 1: 하루 15분 학습 흐름의 최소 수직 슬라이스
- Phase 2: JSON 콘텐츠 구조, 로더, 스키마 및 검증
- Phase 3: 여러 날 세션, 진도, streak, 전체 진도 화면
- Phase 4: 간격 반복 복습과 취약 개념 관리
- Phase 5: 12개 모듈·120개 카탈로그와 사람 검수 기반 콘텐츠 투입
- Phase 6: 선택적 AI 직접 설명 피드백
- Phase 7: 실제 사용 기반 개선, 백업·복원, 안정화

Phase 1부터 Next.js App Router와 TypeScript strict를 사용하고, Phase 1 구현 완료 후 Vercel에
연결할 예정이다.

## 기준 문서

- [공통 작업 규칙](CODEX_MASTER.md)
- [제품 상세 명세](docs/product-spec.md)
- [Phase별 실행 프롬프트](docs/codex-phase-prompts.md)
- [제품·기술 결정 기록](docs/decisions.md)
