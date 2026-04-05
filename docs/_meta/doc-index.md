# 문서 인덱스 (Claude 진입점)
> 최종수정: 2026-04-05 | 상태: 초기화

## 실행 방법
- **사용자:** `USER-GUIDE.md` 읽기
- **에이전트:** `/design-orchestrator` 실행 → Phase 1~7 자동 진행

## 현재 진행 상태

| Phase | 산출물 | 상태 | 위치 |
|-------|--------|------|------|
| 1 | 요구사항 분석/정제 | ⬜ 미시작 | docs/00_srs/ |
| 2 | 기능정의서 | ⬜ 미시작 | docs/01_feature/ |
| 3 | 분석 (프로세스/IA/업무규칙) | ⬜ 미시작 | docs/02_analysis/ |
| 4 | 데이터설계 (ERD/테이블/API) | ⬜ 미시작 | docs/03_data/ |
| 5 | 화면설계 (디자인+화면상세) | ⬜ 미시작 | docs/04_design/, docs/05_screen/ |
| 6 | 품질/아키텍처 | ⬜ 미시작 | docs/06_quality/, docs/07_arch/ |
| 7 | 동기화 + 검증 | ⬜ 미시작 | docs/_meta/ |

## 문서 의존성 맵
```
glossary ──────────→ [모든 문서] (용어 기준)
feature-definition ─→ process, ia, api-spec, screen-index, test-definition
api-spec ──────────→ error-code, SCR-NNN, test-definition
SCR-NNN ───────────→ screen-index, test-definition
erd/table-def ─────→ api-spec, architecture
```

## 가이드 문서
| 문서 | 용도 |
|------|------|
| [USER-GUIDE.md](../../USER-GUIDE.md) | 사용자 매뉴얼 |
| [skill-map.md](skill-map.md) | 설계/개발/운영 스킬 분류 |
| [dev-kickoff-guide.md](dev-kickoff-guide.md) | 설계 완료 후 개발 가이드 |
| [six-principles-guide.md](six-principles-guide.md) | 6원칙 적용 비교 |
