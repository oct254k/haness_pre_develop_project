# 문서 인덱스 (Claude 진입점)
> 최종수정: 2026-04-05 | 상태: 초기화

## 실행 방법
- **사용자:** `USER-GUIDE.md` 읽기
- **에이전트:** `/design-orchestrator` 실행 → Phase 1~9 자동 진행

## 현재 진행 상태

| Phase | 산출물 | 상태 | 위치 |
|-------|--------|------|------|
| 1 | 요구사항 분석/정제 | ⬜ 미시작 | docs/00_srs/ |
| 2 | 기능정의서 | ⬜ 미시작 | docs/01_feature/ |
| 3 | IA (도메인/메뉴 구조) | ⬜ 미시작 | docs/02_analysis/ia.md |
| 4 | 프로세스 + 업무규칙 | ⬜ 미시작 | docs/02_analysis/ |
| 5 | 데이터설계 (ERD/테이블/API/인터페이스) | ⬜ 미시작 | docs/03_data/ |
| 6 | 아키텍처 + 디자인시스템 + ADR | ⬜ 미시작 | docs/07_arch/, docs/04_design/ |
| 7 | 화면설계 | ⬜ 미시작 | docs/05_screen/ |
| 8 | 품질 (TC/에러코드) | ⬜ 미시작 | docs/06_quality/ |
| 9 | 동기화 + 검증 | ⬜ 미시작 | docs/_meta/ |

## 문서 의존성 맵
```
glossary ──────────→ [모든 문서]
nfr-definition ────→ architecture, api-spec(성능), test-definition(NFR TC), SCR-NNN(접근성)
feature-definition ─→ process, ia, api-spec, screen-index, test-definition
ia ────────────────→ process, business-rules, erd, ux-flow, menu→screen
process + biz-rules → screen(프로세스/조건), api-spec(제약), test-definition
erd/table-def ─────→ api-spec, architecture
interface-spec ────→ architecture(연동 구조), api-spec(외부 호출), SCR-NNN(연동 화면)
architecture ──────→ design-system, tech-stack, api-spec(보안/페이징 규약)
design-system ─────→ SCR-NNN (컴포넌트 사용)
ux-flow ───────────→ SCR-NNN (화면 간 이동)
api-spec ──────────→ error-code, SCR-NNN, test-definition
SCR-NNN ───────────→ test-definition, error-code(화면동작)
```

## _meta 파일
| 파일 | 용도 |
|------|------|
| doc-index.md | 진입점 (이 파일) |
| glossary.md | 용어사전 |
| sync-status.md | 문서 동기화 상태 |
| progress-log.md | 실행 기록 (세션 재개용) |

## 가이드
| 문서 | 용도 |
|------|------|
| [USER-GUIDE.md](../../USER-GUIDE.md) | 사용자 매뉴얼 |
