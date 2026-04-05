# 6원칙: 문서 단계 vs 개발 단계 적용 비교
> 최종수정: 2026-04-05 | 상태: 확정

## 결론: 6원칙은 모든 단계에 적용된다. 다만 **구현 방식**이 다르다.

| # | 원칙 | 문서 단계 (현재) | 개발 단계 (다음) |
|---|------|----------------|----------------|
| 1 | **최소 지침** | CLAUDE.md 50줄, rules 3개 | CLAUDE.md에 빌드/테스트 명령 추가, rules에 backend/frontend/testing 추가 |
| 2 | **자동 강제** | Hook: 화면설계서 7섹션 검증, sync 알림 | Hook: 빌드 실패 차단, 테스트 미통과 커밋 차단, lint 자동 실행 |
| 3 | **경량 도구** | 스킬 체인 4개로 문서 생성 | /check-feature, /run-tests 등 슬래시 커맨드 |
| 4 | **점진적 작업** | doc-index.md STEP 추적, 승인 후 다음 | TDD RED→GREEN→REFACTOR, 1 TC = 1 커밋 |
| 5 | **컨텍스트 루프** | doc-index.md 매 세션 첫 읽기 | CLAUDE.md + feature-definition.md 매 세션 주입 |
| 6 | **가비지 청소** | 빈 문서/미등록 파일 삭제 | 데드코드, 미사용 의존성, 고아 테스트 삭제 |

## 현재 하네스 점검 결과

| 원칙 | 문서 단계 구현 | 상태 |
|------|-------------|------|
| 최소 지침 | CLAUDE.md 49줄, rules 짧고 글로브 스코프 | ✅ 적합 |
| 자동 강제 | PreToolUse: 화면설계서 7섹션 검증 / PostToolUse: sync 알림 | ✅ 개선됨 |
| 경량 도구 | /sync-docs, /check-feature, /next-step | ✅ 적합 |
| 점진적 작업 | doc-index.md STEP 0~10 추적표 | ✅ 적합 |
| 컨텍스트 루프 | 메모리 3건 + doc-index 진입점 | ⚠️ SessionStart 훅 없음 (관습 의존) |
| 가비지 청소 | garbage-cleanup.md 규칙 정의됨 | ✅ 적합 |

## 개발 단계 전환 시 변경 사항

```
settings.json 변경:
  permissions.allow += "Write(src/**)", "Bash(npm *)", "Bash(gradle *)"
  hooks += PreCommit: 테스트 통과 검증
  hooks += PostToolUse(Bash): 빌드 결과 확인

CLAUDE.md 변경:
  스킬 체인 → brainstorming → TDD → debugging → review → finish
  빌드 명령, 테스트 명령, 패키지 구조 추가

rules/ 추가:
  backend.md, frontend.md, testing.md, api.md
```
