---
description: 문서 작성 시 ID 채번 및 네이밍 규칙
globs: docs/**/*.md
---

# 문서 네이밍 규칙

## 필수 ID 형식
- 기능: F-{DOMAIN}-{NNN} (예: F-USER-001)
- 화면: SCR-{NNN} (예: SCR-001)
- 업무규칙: BR-{DOMAIN}-{NNN} (예: BR-USER-001)
- TC: TC-{F-ID}-{NNN} (예: TC-USER-001-001)
- ADR: ADR-{NNN}
- 에러: ERR-{DOMAIN}-{NNN}
- NFR: NFR-{카테고리}-{NNN} (예: NFR-PERF-001, NFR-SEC-001)

## 파일명 규칙
- 화면설계서: docs/05_screen/screens/SCR-{NNN}.md
- ADR: docs/07_arch/adr/ADR-{NNN}.md
- 나머지: 폴더별 고정 파일명 사용 (feature-definition.md 등)

## 문서 분할 규칙
양이 많으면 하나의 파일에 다 쓰지 않고 나눈다.

### 분할 기준
| 문서 | 분할 시점 | 분할 방식 |
|------|----------|----------|
| feature-definition | 기능 30개 초과 | 도메인별: feature-definition-{DOMAIN}.md |
| api-spec | 엔드포인트 30개 초과 | 도메인별: api-spec-{DOMAIN}.md |
| table-definition | 테이블 20개 초과 | 도메인별: table-definition-{DOMAIN}.md |
| test-definition | TC 50개 초과 | 기능별: test-definition-{DOMAIN}.md |
| business-rules | 규칙 30개 초과 | 도메인별: business-rules-{DOMAIN}.md |
| process | 프로세스 20개 초과 | 도메인별: process-{DOMAIN}.md |
| 화면설계서 | 항상 | 화면당 1파일: SCR-{NNN}.md (이미 분리됨) |

### 분할 시 필수
- 원본 파일에 인덱스(목차 + 파일 링크) 유지
- docs/_meta/sync-status.md에 분할 이력 기록
- glossary.md 용어는 분할 파일에서도 동일하게 적용

## 입력물 (docs/00_srs/)
- PDF → `/pdf` 스킬로 직접 읽기 가능 (수동 추출 불필요)
- 여러 파일 가능: RFP.pdf, 미팅노트.md, 기획서.md 등 혼합 OK
- 원문은 수정하지 않음 (정제본을 별도 생성)

## 금지
- ID 없는 기능/화면 정의
- 폴더 규칙 벗어나는 파일 생성
- 분할 기준 초과했는데 한 파일에 계속 쓰기
