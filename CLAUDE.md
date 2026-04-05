# Harness: 설계 문서 자동화

## 프로젝트 목적
RFP/고객요구 → 정식 SRS → 개발 산출물 전체를 Claude Code 스킬 체인으로 생성한다.

## 스킬 체인 (순서 엄수)
1. `requirements-preprocessor` → 비정형 요구 정제
2. `requirements-analyzer` → SRS 6개 품질기준 검증
3. `doc-orchestrator` → 10단계 문서 생성 (STEP 0~10)
4. `doc-sync` → 문서 간 동기화

## 진입점
- 문서 탐색: `docs/_meta/doc-index.md`
- 동기화 상태: `docs/_meta/sync-status.md`

## ID 규칙
- 기능: `F-{도메인}-{NNN}` (예: F-USER-001)
- 화면: `SCR-{NNN}` (예: SCR-001)
- TC: `TC-{기능ID}-{NNN}` (예: TC-USER-001-001)
- API: `{METHOD} /api/v1/{resource}`
- ADR: `ADR-{NNN}`

## 6원칙 (하네스 기둥)
1. **최소 지침** - .md는 짧게, AI가 읽을 수 있게만
2. **자동 강제** - Hook으로 실수 차단
3. **경량 도구** - 무거운 도구 대신 CLI 커맨드
4. **점진적 작업** - 한 STEP씩, 승인 후 다음
5. **컨텍스트 루프** - doc-index.md를 항상 먼저 읽기
6. **가비지 청소** - 사용 안 하는 산출물은 삭제

## 문서 구조
```
docs/
├── 00_srs/          # 입력 (RFP, SRS)
├── 01_feature/      # 기능정의
├── 02_analysis/     # 프로세스, IA, 업무규칙
├── 03_data/         # ERD, 테이블, API
├── 04_design/       # 디자인시스템, 와이어프레임
├── 05_screen/       # 화면설계서
├── 06_quality/      # TC, 에러코드
├── 07_arch/         # 아키텍처, ADR
└── _meta/           # doc-index, sync-status
```

## 금지
- docs/ 외부에 문서 생성 금지
- ID 없는 기능/화면 정의 금지
- sync-status 갱신 없이 문서 수정 금지
