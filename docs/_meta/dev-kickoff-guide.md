# 개발 킥오프 가이드
> 최종수정: 2026-04-05 | 작성: 하네스 | 상태: 초안

## 목적
설계 문서(docs/) 완성 후, 새 프로젝트에서 문서를 읽고 개발을 시작하는 절차.

---

## 1단계: 문서 읽기 순서 (필수)

### 먼저 읽기 (10분)
```
docs/_meta/doc-index.md      ← 전체 구조 파악
docs/_meta/sync-status.md    ← 완성도 확인
```

### 요구사항 이해 (1시간)
```
docs/01_feature/feature-definition.md  ← 기능 목록 + ID (F-XXX-NNN)
docs/02_analysis/business-rules.md     ← 업무규칙 (위반 불가)
docs/02_analysis/process.md            ← 사용자 플로우
```

### 데이터/API 이해 (1시간)
```
docs/03_data/erd.md                    ← 엔티티 관계
docs/03_data/table-definition.md       ← 컬럼명/타입 (코드에 그대로 사용)
docs/03_data/api-spec.md               ← 엔드포인트 계약서
```

### UI/테스트 이해 (필요 시)
```
docs/05_screen/screen-index.md         ← 화면 목록
docs/05_screen/screens/SCR-NNN.md      ← 해당 화면 상세
docs/06_quality/test-definition.md     ← TC = 인수조건
docs/06_quality/error-code.md          ← 에러코드 표준
docs/07_arch/architecture.md           ← 레이어 구조
```

---

## 2단계: 개발 하네스 세팅

### CLAUDE.md 전환
설계용 CLAUDE.md를 개발용으로 교체한다:
- 스킬 체인: `brainstorming → TDD → debugging → review → finish`
- permissions: `Write(src/**)` 허용, `Write(docs/**)` 유지
- hooks: 빌드/테스트 자동 실행 추가

### 개발용 rules/ 추가
```
.claude/rules/
├── doc-naming.md          ← 유지
├── korean-si-standard.md  ← 유지
├── garbage-cleanup.md     ← 유지
├── backend.md             ← 새로 생성 (패키지 구조, 네이밍)
├── frontend.md            ← 새로 생성 (컴포넌트 규칙)
├── testing.md             ← 새로 생성 (TDD 규칙)
└── api.md                 ← 새로 생성 (REST 규칙)
```

### 개발용 commands/ 추가
```
.claude/commands/
├── sync-docs.md           ← 유지
├── check-feature.md       ← 유지
├── next-step.md           ← 유지
└── run-tests.md           ← 새로 생성
```

---

## 3단계: 스킬 활성화 순서

### Tier 1: 코딩 전 (항상)
| 순서 | 스킬 | 용도 | 트리거 |
|------|------|------|--------|
| 1 | brainstorming | 설계 검증 후 구현 시작 | "F-XXX-NNN 어떻게 구현할까?" |
| 2 | doc-sync | 문서 최신 상태 확인 | 세션 시작 시 |

### Tier 2: 코딩 중 (핵심 루프)
| 순서 | 스킬 | 용도 | 트리거 |
|------|------|------|--------|
| 3 | test-driven-development | RED→GREEN→REFACTOR | 기능 구현 시 |
| 4 | systematic-debugging | 테스트 실패 시 근본원인 | 에러 발생 시 |
| 5 | test-generator | TC → 테스트코드 자동생성 | TC 추가 시 |

### Tier 3: 코딩 후 (머지 전)
| 순서 | 스킬 | 용도 | 트리거 |
|------|------|------|--------|
| 6 | requesting-code-review | 자기검토 체크리스트 | PR 전 |
| 7 | finishing-dev-branch | 최종 검증 + 커밋 정리 | "완료", "PR 올릴게" |
| 8 | doc-sync | API 변경 → 문서 동기화 | 커밋 후 |

---

## 4단계: 일일 개발 루틴

```
┌─ 세션 시작 (10분)
│  ├─ doc-index.md 읽기
│  └─ /check-feature F-XXX-NNN
│
├─ 브레인스토밍 (5-15분) ← brainstorming
│  └─ 설계 확인 → 승인
│
├─ TDD 루프 (핵심) ← test-driven-development
│  ├─ RED: 실패 테스트 작성
│  ├─ GREEN: 최소 코드로 통과
│  ├─ REFACTOR: 정리
│  └─ COMMIT: feat(F-XXX-NNN): 설명
│  (TC별 반복)
│
├─ 리뷰 (30분) ← code-review + finishing
│  └─ 셀프 리뷰 → 최종 검증 → PR/머지
│
└─ 문서 동기화 ← doc-sync
   └─ sync-status.md 갱신
```

---

## 5단계: 운영 전환

모든 기능 sync-status.md ✅ 완료 시:
1. `handover-orchestrator` 실행 → docs-ops/ 생성
2. CLAUDE.md 운영 모드로 전환
3. `maintenance-orchestrator`로 운영 시작
