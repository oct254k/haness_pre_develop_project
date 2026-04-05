# 사용자 가이드

## 이 하네스가 뭔가요?
RFP나 요구사항을 넣으면, Claude가 개발에 필요한 모든 설계 문서를 단계별로 만들어줍니다.

---

## 빠른 시작 (3분)

### 1. 입력물 준비

**필수: 요구사항** → `docs/00_srs/`에 넣기
- PDF 그대로 OK (에이전트가 `/pdf` 스킬로 직접 읽음)
- .md, .txt 등 텍스트 파일도 OK
- 여러 파일 혼합 가능 (RFP.pdf + 미팅노트.md + 기획서.pdf)
- 원문은 건드리지 않음 (정제본은 별도 생성)

**선택: 디자인 템플릿** → Phase 6에서 요청됨
- Figma 링크, HTML 템플릿, CSS 파일, 기존 사이트 URL 등
- 있으면: 템플릿에서 색상/타이포/컴포넌트 자동 추출
- 없으면: 기술 스택 기반 기본 디자인시스템 생성 (반응형 기본)

### 2. RFP 정제
```
"/prepare-rfp"
```
→ 복붙 .md + 원본 PDF를 병렬로 교차검증 → `docs/00_srs/rfp-final.md` 생성

### 3. 설계 파이프라인 실행
```
"/design-orchestrator"
```

### 4. 끝. 나머지는 에이전트가 한다.
- Phase별로 진행하며 중간중간 승인 요청
- 승인하면 다음 Phase로 넘어감
- 전체 완료 후 검증 리포트 제공

---

## 사용 가능한 명령어

| 명령어 | 언제 쓰나 | 뭘 하나 |
|--------|----------|---------|
| `/prepare-rfp` | **가장 먼저** | PDF+복붙 교차검증 → rfp-final.md 생성 |
| `/study-rfp` | RFP 정제 후 (선택) | 도메인 강의자료 생성 (공부용) |
| `/design-orchestrator` | RFP 정제 후 | 전체 설계 파이프라인 실행 (Phase 1~9) |
| `/next-step` | 현재 진행 상태 궁금할 때 | 어디까지 했는지, 다음에 뭘 해야 하는지 |
| `/check-feature F-XXX-NNN` | 특정 기능 추적 확인 | 기능→화면→API→TC 연결 검증 |
| `/sync-docs` | 문서 수정 후 | 연관 문서 동기화 상태 점검 |

---

## Phase별 진행 + 생성되는 문서

### Phase 1: 요구사항 분석
- **내가 할 일:** 불명확한 부분 질문에 답변
- **생성 문서:**
  - `docs/00_srs/requirements-refined.md` — 정제된 요구사항
  - `docs/00_srs/nfr-definition.md` — 비기능 요구사항 (성능/보안/접근성)
  - `docs/_meta/glossary.md` — 용어사전 (갱신)

### Phase 2: 기능 정의
- **내가 할 일:** 빠진 기능/우선순위 확인, 추가 질문에 답변
- **생성 문서:**
  - `docs/01_feature/feature-definition.md` — 전체 기능 목록 (F-ID)
  - 기능 30개+ 시 도메인별 분리

### Phase 3: IA (도메인/메뉴 구조)
- **내가 할 일:** 도메인 경계와 메뉴 구조 맞는지 확인
- **생성 문서:**
  - `docs/02_analysis/ia.md` — 도메인 구조, 메뉴, 네비게이션

### Phase 4: 프로세스 + 업무규칙
- **내가 할 일:** 제약조건(순서/권한/정합성)이 현실과 맞는지
- **생성 문서:**
  - `docs/02_analysis/process.md` — 업무 플로우, 제약조건
  - `docs/02_analysis/business-rules.md` — 비즈니스 룰 (BR-ID)

### Phase 5: 데이터 설계
- **내가 할 일:** ERD, 테이블, API 구조 검토
- **생성 문서:**
  - `docs/03_data/erd.md` — 엔티티 관계도
  - `docs/03_data/table-definition.md` — 테이블 정의
  - `docs/03_data/api-spec.md` — API 스펙 (엔드포인트, 요청/응답)

### Phase 6: 아키텍처 + 디자인시스템
- **내가 할 일:** 기술 스택 승인, 디자인 템플릿 제공 (선택)
- **생성 문서:**
  - `docs/07_arch/architecture.md` — 시스템 아키텍처, 보안 설계, API 규약
  - `docs/07_arch/tech-stack.md` — 기술 스택 + 근거
  - `docs/04_design/design-system.md` — 색상, 타이포, 컴포넌트, 반응형

### Phase 7: 화면 설계
- **내가 할 일:** 화면 흐름, 빠진 케이스 확인
- **생성 문서:**
  - `docs/04_design/ux-flow.md` — 화면 간 이동 경로
  - `docs/05_screen/screen-index.md` — 화면 목록
  - `docs/05_screen/screens/SCR-NNN.md` — 화면당 1파일 (와이어프레임, 상호작용, 프로세스, API, 엣지케이스)

### Phase 8: 품질
- **내가 할 일:** 테스트 범위 확인
- **생성 문서:**
  - `docs/06_quality/test-definition.md` — 테스트 케이스 (TC-ID)
  - `docs/06_quality/error-code.md` — 에러코드 레지스트리

### Phase 9: 검증
- **내가 할 일:** 검증 리포트 확인, 문제 있으면 수정 요청
- **갱신 문서:**
  - `docs/_meta/sync-status.md` — 전체 동기화 상태
  - `docs/_meta/doc-index.md` — 진행 상태 ✅ 갱신

---

## 전체 문서 목록 (완료 시)

```
docs/
├── 00_srs/
│   ├── (원문 입력물)           ← 내가 넣은 것
│   ├── requirements-refined.md ← 정제된 요구사항
│   └── nfr-definition.md      ← 비기능 요구사항
├── 01_feature/
│   └── feature-definition.md   ← 기능 목록 (F-ID)
├── 02_analysis/
│   ├── ia.md                   ← 도메인/메뉴/네비게이션
│   ├── process.md              ← 업무 플로우
│   └── business-rules.md       ← 비즈니스 룰 (BR-ID)
├── 03_data/
│   ├── erd.md                  ← 엔티티 관계도
│   ├── table-definition.md     ← 테이블 정의
│   └── api-spec.md             ← API 스펙
├── 04_design/
│   ├── design-system.md        ← 디자인시스템 (색상/타이포/컴포넌트)
│   └── ux-flow.md              ← 화면 간 이동 경로
├── 05_screen/
│   ├── screen-index.md         ← 화면 목록
│   └── screens/SCR-NNN.md      ← 화면당 상세 설계
├── 06_quality/
│   ├── test-definition.md      ← 테스트 케이스 (TC-ID)
│   └── error-code.md           ← 에러코드 (ERR-ID)
├── 07_arch/
│   ├── architecture.md         ← 시스템 아키텍처 + 보안 + API 규약
│   └── tech-stack.md           ← 기술 스택
└── _meta/
    ├── doc-index.md            ← 진입점 (진행 상태)
    ├── glossary.md             ← 용어사전
    └── sync-status.md          ← 동기화 상태
```

---

## 내가 해야 할 것 vs 에이전트가 할 것

| 나 (사용자) | 에이전트 |
|------------|---------|
| 요구사항 입력물 준비 | 요구사항 분석/정제 |
| Phase 완료 시 승인/수정 요청 | 문서 생성 (병렬 서브에이전트) |
| 업무 지식 질문에 답변 | 문서 간 동기화 |
| 디자인 템플릿 제공 (선택) | 검증 리포트 생성 |

---

## 문서 수정하고 싶을 때

### 특정 문서만 수정
```
"docs/05_screen/screens/SCR-001.md에서 저장 버튼 조건 바꿔줘.
저장 전에 이메일 중복체크가 먼저 되어야 해"
```
→ 에이전트가 수정 + 연관 문서 동기화 알림

### 특정 Phase부터 재실행
```
"/design-orchestrator Phase 5부터 다시 해줘"
```

---

## 문제 생기면

| 상황 | 해결 |
|------|------|
| 에이전트가 멈춤 | `/next-step`으로 현재 상태 확인 |
| 문서가 이상함 | 직접 수정 후 `/sync-docs` |
| 특정 기능 누락 의심 | `/check-feature F-XXX-NNN` |
| 처음부터 다시 | `docs/` 폴더 비우고 `/design-orchestrator` |

---

## 설계 완료 후

설계 문서가 모두 완성되면 → 개발 하네스를 별도 세팅하여 개발 시작.
이 문서들을 읽고 코딩하는 가이드는 개발 하네스에서 제공됩니다.
