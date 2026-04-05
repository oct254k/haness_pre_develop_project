설계 문서 전체 파이프라인을 Phase → Step 순서로 실행한다.
각 Phase 완료 시 사용자에게 리포트 + 승인을 요청하고, 승인 후 다음 Phase로 진행한다.

$ARGUMENTS 가 있으면 해당 Phase부터 시작한다. (예: "Phase 3부터")

---

## 핵심 철학: 병렬 에이전트 실행

> 문서를 나누는 이유는 **서브에이전트가 병렬로 작업하기 위함**이다.
> 1파일 = 1에이전트가 담당할 수 있는 단위.
> 독립적인 작업은 반드시 서브에이전트로 동시 실행한다.

### 병렬 실행 원칙
- **작성 병렬화**: 도메인별로 분리된 파일은 서브에이전트가 동시에 작성
- **검증 병렬화**: 문서별 검증도 서브에이전트가 동시에 수행
- **동기화는 병렬 작업 완료 후**: 모든 서브에이전트 결과를 모아서 교차 검증
- **의존성 있는 작업만 순차**: Phase 간은 순차, Phase 내 독립 Step은 병렬

---

## 실행 전 필수 확인
1. docs/_meta/doc-index.md 읽기 (현재 진행 상태 확인)
2. docs/_meta/glossary.md 읽기 (용어 기준 확인)
3. docs/00_srs/ 에 입력물 존재 확인 (없으면 사용자에게 요청)

---

## Phase 1: 요구사항 분석 및 정제
> ⚡ 순차 실행 (입력물 분석은 전체 맥락 필요)

### Step 1.1: 입력물 읽기
- docs/00_srs/ 의 모든 파일 읽기 (PDF는 /pdf 스킬 사용)
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
✅ 도메인 식별: {도메인 목록} ← Phase 2 병렬 분할의 기준
⚠️ 미확인: {N}개 (추후 확인 가능)
📄 생성/갱신: requirements-refined.md, glossary.md
→ 승인하시겠습니까? (승인 / 수정 요청 / 중단)
```

---

## Phase 2: 기능 정의
> ⚡ 병렬: 도메인별 서브에이전트가 동시에 기능 도출

### Step 2.1: 도메인별 분할 준비
- Phase 1에서 식별된 도메인 목록 확인
- 도메인 수 만큼 서브에이전트 할당 계획

### Step 2.2: 기능 도출 (🔀 병렬)
- **서브에이전트 N개 동시 실행** (도메인당 1���)
- 각 서브에이전트:
  - glossary.md + requirements-refined.md 읽기
  - 해당 도메인 기능 추출
  - F-{DOMAIN}-{NNN} ID 채번
  - feature-definition-{DOMAIN}.md 생성
- 기능 30개 이하면 feature-definition.md 단일 파일로 통합

### Step 2.3: 병합 + 우선순위
- 서브에이전트 결과 수집
- 전체 기능 목록 통합, 중복 제거
- 우선순위 (P1/P2/P3) 배정

### Step 2.4: 추가 인풋 수집
- 사용자에게 질문:
  - Q1: 외부 연동 API가 있나?
  - Q2: 도메인 특수 개념/용어가 있나?
  - Q3: 레거시 데이터 이관이 필요한가?
  - Q4: 빠진 기능이 있나?
  - Q5: 기술 제약이 있나?

### Phase 2 리포트
```
✅ 기능: {N}개 도출 (P1: {n}개, P2: {n}개, P3: {n}개)
🔀 병렬 실행: {N}개 도메인 × 서브에이전트
📄 생성: feature-definition.md (또는 도메인별 분리)
→ 승인하시겠습니까?
```

---

## Phase 3: 분석 (프로세스 / IA / 업무규칙)
> ⚡ 병렬: ia.md / process.md / business-rules.md 동시 작성

### Step 3.1 ~ 3.3 (��� 병렬 — 서브에이전트 3개 동시)

**에이전트 A: IA**
- glossary.md + feature-definition 참조
- 도메인 정리: 각 도메인의 존재 이유, 경계, 관계
- 메뉴 구조: 각 메뉴의 목적, 접근 권한 조건
- docs/02_analysis/ia.md 생성

**에이전트 B: 프로세스**
- glossary.md + feature-definition 참조
- 주요 업무 플로우 (사용자 시나리오)
- 프로세스별 제약조건: 순서 의존성, 데이터 정합성, 동시성, 권한
- docs/02_analysis/process.md 생성
- 양 많으면 도메인별 분리: process-{DOMAIN}.md

**에이전트 C: 업무규칙**
- glossary.md + feature-definition 참조
- 비즈니스 룰 목록 (위반 불가 규칙)
- 각 규칙의 적용 범위와 예외 조건
- docs/02_analysis/business-rules.md 생성
- 양 많으면 도메인별 분리: business-rules-{DOMAIN}.md

### Step 3.4: 교차 검증 (병렬 완료 후)
- 3개 문서 간 정합성 확인
- IA 도메인 ↔ 프로세스 도메인 ↔ 업무규칙 도메인 일치 여부
- 불일치 있으면 수정

### Phase 3 리포트
```
✅ 도메인: {N}개 | 프로세스: {N}개 | 업무규칙: {N}개
🔀 병렬 실행: 서브에이전트 3개 (IA / 프로세스 / 업무규칙)
📄 생성: ia.md, process.md, business-rules.md
⚠️ 확인 필요: 제약조건이 현실과 맞는지 검토 요청
→ 승인하시겠습니까?
```

---

## Phase 4: 데이터 설계
> ⚡ ERD 먼저 (순차) → 테이블/API 병렬

### Step 4.1: ERD (순차 — 전체 구조 먼저)
- 엔티티 관계도 (Mermaid erDiagram)
- docs/03_data/erd.md 생성

### Step 4.2 ~ 4.3 (🔀 병렬 — ERD 완료 후)

**에이전트 A: 테이블 정의**
- erd.md 기반으로 컬럼명, 타입, 제약조건, 인덱스
- docs/03_data/table-definition.md 생성
- 테이블 20개 초과 시 도메인별 분리

**에이전트 B: API 스펙**
- feature-definition + erd.md 기반
- 엔드포인트, 메서드, 요청/응답, 에러코드
- docs/03_data/api-spec.md 생성
- 엔드포인트 30개 초과 시 도메인별 분리

### Phase 4 리포트
```
✅ 테이블: {N}개 | API: {N}개 엔드포인트
🔀 병렬 실행: 서브에이전트 2개 (테이블 / API)
📄 생성: erd.md, table-definition.md, api-spec.md
→ 승인하시겠습니까?
```

---

## Phase 5: 화면 설계
> ⚡ 가장 병렬화가 큰 Phase — 화면당 1에이전트

### Step 5.1: 디자인 기반 (순차 — 공통 기준 먼저)
- 디자인 시스템 (색상, 타이포, 컴포넌트 패턴)
- UX 플로우 (화면 간 이동 경로)
- docs/04_design/design-system.md, ux-flow.md 생성

### Step 5.2: 화면 목록 (순차)
- 전체 화면 리스트 + 기능 매핑
- docs/05_screen/screen-index.md 생성

### Step 5.3: 화면별 상세 (🔀 대규모 병렬 — 화면당 1 서브에이전트)
- **서브에이전트 N개 동시 실행** (화면당 1개)
- 각 서브에이전트가 읽을 공통 컨텍스트:
  - glossary.md, design-system.md, ux-flow.md
  - feature-definition (해당 기능), api-spec (해당 API)
- 각 SCR-NNN.md에 포함:
  1. **화면 목적** — 왜 존재하나, 사용자가 뭘 하려는가
  2. **와이어프레임** — ASCII/Mermaid 레이아웃
  3. **상세 기능** — 모든 요소, 데이터타입, 기본값, 필수여부
  4. **�����작용** — 트리거 → 결과 매핑
  5. **화면 내 프로세스** — 버튼 조건분기, 상태전이, 조건부 표시/숨김
  6. **API 연동** — 호출시점, 엔드포인트, 에러처리
  7. **엣지 케이스** — 빈��이터, 권한없음, 네트워크에러

### Phase 5 리포트
```
✅ 화면: {N}개 생성됨
🔀 병렬 실행: 서브에이전트 {N}개 (화면당 1개)
📄 생성: screen-index.md + SCR-001 ~ SCR-{N}.md
→ 승인하시겠습니까?
```

---

## Phase 6: 품질 / 아키텍처
> ⚡ 병렬: TC/에러코드/아키텍처 동시 작성

### Step 6.1 ~ 6.3 (🔀 병렬 — 서브에이전트 3개)

**에이전트 A: 테스트 케이스**
- feature-definition + SCR-NNN + api-spec 참조
- 기능별 TC 정의 (TC-{F-ID}-{NNN})
- docs/06_quality/test-definition.md 생성
- TC 50개 초과 시 도메인별 분리

**에이전트 B: 에러코드**
- api-spec + business-rules 참조
- 에러코드 레지스트리 (ERR-{DOMAIN}-{NNN})
- docs/06_quality/error-code.md 생성

**에이전트 C: 아키텍처**
- feature-definition + erd + api-spec + process 참조
- 시스템 아키텍처 (레이어, 패키지 구조)
- 기술 스택 결정 + 근거
- docs/07_arch/architecture.md, tech-stack.md 생성

### Phase 6 리포트
```
✅ TC: {N}개 | 에러코드: {N}개
🔀 병렬 실행: 서브에이전트 3개 (TC / 에러코드 / 아키텍처)
📄 생성: test-definition.md, error-code.md, architecture.md, tech-stack.md
→ 승인하시겠습니까?
```

---

## Phase 7: 동기화 + 검증
> ⚡ 검증도 병렬 — 문서별 서브에이전트가 동시에 체크

### Step 7.1: 교차 동기화 (🔀 병렬 검증)
- **서브에이전트 N개가 각 문서를 동시에 검증:**
  - 에이전트 A: feature ↔ screen 매핑 검증
  - 에이전트 B: feature ↔ API 매핑 검증
  - 에이전트 C: feature ↔ TC 매핑 검증
  - 에이전트 D: API ↔ error-code 매핑 검증
  - 에이전트 E: glossary 용어 일관성 검증 (모든 문서 스캔)
- 각 에이전트가 누락/불일치 목록 반환

### Step 7.2: 불일치 수정
- 서브에이전트 결과 병합
- 누락된 연결 추가, 불일치 수정
- glossary.md 갱신

### Step 7.3: 품질 검증
- 각 문서가 "읽고 바로 코딩 가능한가?" 기준 검토
- 빈 섹션, 모호한 표현, 미완성 항목 식별

### Step 7.4: sync-status.md 갱신
- 전체 문서 상태 업데이트
- doc-index.md Phase 상태 ✅로 갱신

### Step 7.5: 검증 리포트
```
=== 설계 문서 검증 리포트 ===

📊 전체 현황
- 기능: {N}개 (P1: {n} / P2: {n} / P3: {n})
- 화면: {N}개
- API: {N}개 엔드포인트
- TC: {N}개
- 에러코드: {N}개

🔀 병렬 검증 결과
- 기능 → 화면 매핑: {N}/{N} ({%})
- 기능 → API 매핑: {N}/{N} ({%})
- 기능 → TC 매핑: {N}/{N} ({%})
- API → 에러코드 매핑: {N}/{N} ({%})
- 용어 일관성: {N}개 불일치 → 수정됨

⚠️ 미해결 항목
- {있으면 나열, 없으면 "없음"}

📁 생성된 파일 목록
{전체 파일 목록}

→ 설계 완료!
```

---

## 에러 처리

### 입력물 없을 때
→ "docs/00_srs/에 요구사항 파일을 넣어주세요" 안내 후 대기

### 사용자가 "수정" 요청 시
→ 해당 Phase의 Step으로 돌아가 수정 후 리포트 재생성

### 사용자가 "중단" 시
→ 현재까지 진행 상태를 doc-index.md에 저장, 나중에 이어서 가능

### 파일 분할 기준 (병렬 실행 단위)
| 문서 | 분할 시점 | 방식 |
|------|----------|------|
| feature-definition | 기능 30개+ | 도메인별 분리 |
| api-spec | 엔드포인트 30개+ | 도메인별 분리 |
| table-definition | 테이블 20개+ | 도메인별 분리 |
| test-definition | TC 50개+ | 도메인별 분리 |
| process | 프로세스 20개+ | 도메인별 분리 |
| business-rules | 규칙 30개+ | 도메인별 분리 |
| 화면설계서 | 항상 | 화면당 1파일 (SCR-NNN.md) |
