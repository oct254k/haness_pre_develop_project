설계 문서 전체 파이프라인을 Phase → Step 순서로 실행한다.
각 Phase 완료 시 사용자에게 리포트 + 승인을 요청하고, 승인 후 다음 Phase로 진행한다.

$ARGUMENTS 가 있으면 해당 Phase부터 시작한다. (예: "Phase 3부터")

---

## 실행 전 필수 확인
1. docs/_meta/doc-index.md 읽기 (현재 진행 상태 확인)
2. docs/_meta/glossary.md 읽기 (용어 기준 확인)
3. docs/00_srs/ 에 입력물 존재 확인 (없으면 사용자에게 요청)

---

## Phase 1: 요구사항 분석 및 정제

### Step 1.1: 입력물 읽기
- docs/00_srs/ 의 모든 파일 읽기
- 요구사항 항목 추출 (기능/비기능 분류)

### Step 1.2: 불명확 항목 식별
- 모호한 표현, 누락 의심 항목, 충돌하는 요구사항 목록화
- 사용자에게 질문 (최대 5개씩 묶어서)

### Step 1.3: 용어 추출
- 입력물에서 도메인 용어, 약어, 동의어 추출
- 사용자에게 확인: "같은 뜻인데 다른 단어 쓰이는 것 있나요?"
- docs/_meta/glossary.md 갱신

### Step 1.4: 요구사항 정제
- 사용자 답변 반영하여 구조화
- glossary.md 용어로 통일하여 작성
- docs/00_srs/requirements-refined.md 생성

### Phase 1 리포트
```
✅ 완료 항목: {N}개 요구사항 정제됨
✅ 용어사전: {N}개 용어 등록
⚠️ 미확인: {N}개 (추후 확인 가능)
📄 생성/갱신: requirements-refined.md, glossary.md
→ 승인하시겠습니까? (승인 / 수정 요청 / 중단)
```

---

## Phase 2: 기능 정의

### Step 2.1: 기능 도출
- glossary.md 용어로 통일하여 작성 (새 용어 발견 시 glossary 먼저 갱신)
- 정제된 요구사항에서 기능 추출
- F-{DOMAIN}-{NNN} ID 채번
- 우선순위 (P1/P2/P3) 배정

### Step 2.2: 기능정의서 작성
- docs/01_feature/feature-definition.md 생성
- 기능별: ID, 이름, 설명, 우선순위, 관련 요구사항 ID

### Step 2.3: 추가 인풋 수집
- 사용자에게 질문:
  - Q1: 외부 연동 API가 있나?
  - Q2: 도메인 특수 개념/용어가 있나?
  - Q3: 레거시 데이터 이관이 필요한가?
  - Q4: 빠진 기능이 있나?
  - Q5: 기술 제약이 있나?

### Phase 2 리포트
```
✅ 기능: {N}개 도출 (P1: {n}개, P2: {n}개, P3: {n}개)
📄 생성: docs/01_feature/feature-definition.md
→ 승인하시겠습니까?
```

---

## Phase 3: 분석 (프로세스 / IA / 업무규칙)

### Step 3.1: 도메인 및 IA
- glossary.md 참조: 도메인 용어가 사전과 일치하는지 확인 (새 용어 → glossary 갱신)
- 도메인 정리: 각 도메인의 존재 이유, 경계, 관계
- 메뉴 구조: 각 메뉴의 목적, 접근 권한 조건
- docs/02_analysis/ia.md 생성

### Step 3.2: 프로세스 정의
- 주요 업무 플로우 (사용자 시나리오)
- 프로세스별 제약조건: 순서 의존성, 데이터 정합성, 동시성, 권한
- docs/02_analysis/process.md 생성

### Step 3.3: 업무규칙
- 비즈니스 룰 목록 (위반 불가 규칙)
- 각 규칙의 적용 범위와 예외 조건
- docs/02_analysis/business-rules.md 생성

### Phase 3 리포트
```
✅ 도메인: {N}개 | 프로세스: {N}개 | 업무규칙: {N}개
📄 생성: ia.md, process.md, business-rules.md
⚠️ 확인 필요: 제약조건이 현실과 맞는지 검토 요청
→ 승인하시겠습니까?
```

---

## Phase 4: 데이터 설계

### Step 4.1: ERD
- 엔티티 관계도 (Mermaid erDiagram)
- docs/03_data/erd.md 생성

### Step 4.2: 테이블 정의
- 컬럼명, 타입, 제약조건, 인덱스
- docs/03_data/table-definition.md 생성

### Step 4.3: API 스펙
- 엔드포인트, 메서드, 요청/응답, 에러코드
- 기능 ID ↔ API 매핑
- docs/03_data/api-spec.md 생성

### Phase 4 리포트
```
✅ 테이블: {N}개 | API: {N}개 엔드포인트
📄 생성: erd.md, table-definition.md, api-spec.md
→ 승인하시겠습니까?
```

---

## Phase 5: 화면 설계

### Step 5.1: 디자인 기반
- 디자인 시스템 (색상, 타이포, 컴포넌트 패턴)
- UX 플로우 (화면 간 이동 경로)
- docs/04_design/design-system.md, ux-flow.md 생성

### Step 5.2: 화면 목록
- 전체 화면 리스트 + 기능 매핑
- docs/05_screen/screen-index.md 생성

### Step 5.3: 화면별 상세
- 각 SCR-NNN.md에 포함할 내용:
  1. **화면 목적** — 왜 존재하나, 사용자가 뭘 하려는가
  2. **와이어프레임** — ASCII/Mermaid 레이아웃
  3. **상세 기능** — 모든 요소, 데이터타입, 기본값, 필수여부
  4. **상호작용** — 트리거 → 결과 매핑
  5. **화면 내 프로세스** — 버튼 조건분기, 상태전이, 조건부 표시/숨김
  6. **API 연동** — 호출시점, 엔드포인트, 에러처리
  7. **엣지 케이스** — 빈데이터, 권한없음, 네트워크에러
- docs/05_screen/screens/SCR-NNN.md 생성

### Phase 5 리포트
```
✅ 화면: {N}개 생성됨
📄 생성: screen-index.md + SCR-001 ~ SCR-{N}.md
→ 승인하시겠습니까?
```

---

## Phase 6: 품질 / 아키텍처

### Step 6.1: 테스트 케이스
- 기능별 TC 정의 (TC-{F-ID}-{NNN})
- docs/06_quality/test-definition.md 생성

### Step 6.2: 에러코드
- 에러코드 레지스트리 (ERR-{DOMAIN}-{NNN})
- docs/06_quality/error-code.md 생성

### Step 6.3: 아키텍처
- 시스템 아키텍처 (레이어, 패키지 구조)
- 기술 스택 결정 + 근거
- docs/07_arch/architecture.md, tech-stack.md 생성

### Phase 6 리포트
```
✅ TC: {N}개 | 에러코드: {N}개
📄 생성: test-definition.md, error-code.md, architecture.md, tech-stack.md
→ 승인하시겠습니까?
```

---

## Phase 7: 동기화 + 검증

### Step 7.1: 교차 동기화
- 모든 문서 간 ID 참조 일치 확인
- 기능 → 화면 → API → TC 추적성 검증
- 누락된 연결 식별

### Step 7.2: 용어 일관성 검증
- 모든 문서에서 glossary.md에 없는 도메인 용어 탐지
- 같은 개념에 다른 단어를 쓴 곳 식별
- glossary.md 갱신 + 문서 내 용어 통일

### Step 7.3: 품질 검증
- 각 문서가 "읽고 바로 코딩 가능한가?" 기준 검토
- 빈 섹션, 모호한 표현, 미완성 항목 식별

### Step 7.3: sync-status.md 갱신
- 전체 문서 상태 업데이트
- doc-index.md STEP 상태 ✅로 갱신

### Step 7.4: 검증 리포트
```
=== 설계 문서 검증 리포트 ===

📊 전체 현황
- 기능: {N}개 (P1: {n} / P2: {n} / P3: {n})
- 화면: {N}개
- API: {N}개 엔드포인트
- TC: {N}개
- 에러코드: {N}개

✅ 추적성 검증
- 기능 → 화면 매핑: {N}/{N} (100%)
- 기능 → API 매핑: {N}/{N} (100%)
- 기능 → TC 매핑: {N}/{N} (100%)

⚠️ 미해결 항목
- {있으면 나열, 없으면 "없음"}

📁 생성된 파일 목록
- docs/00_srs/requirements-refined.md
- docs/01_feature/feature-definition.md
- docs/02_analysis/ia.md, process.md, business-rules.md
- docs/03_data/erd.md, table-definition.md, api-spec.md
- docs/04_design/design-system.md, ux-flow.md
- docs/05_screen/screen-index.md + SCR-NNN.md x {N}
- docs/06_quality/test-definition.md, error-code.md
- docs/07_arch/architecture.md, tech-stack.md
- docs/_meta/sync-status.md (갱신됨)

→ 설계 완료! 개발 시작은 docs/_meta/dev-kickoff-guide.md 참고
```

---

## 에러 처리

### 입력물 없을 때
→ "docs/00_srs/에 요구사항 파일을 넣어주세요" 안내 후 대기

### 사용자가 "수정" 요청 시
→ 해당 Phase의 Step으로 돌아가 수정 후 리포트 재생성

### 사용자가 "중단" 시
→ 현재까지 진행 상태를 doc-index.md에 저장, 나중에 이어서 가능

### 파일 크기 초과 시 (50개 이상 기능)
→ 도메인별 분리: feature-definition-{DOMAIN}.md
→ split-log.md에 분리 이력 기록
