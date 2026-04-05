---
description: 문서 작성 시 ID 채번 및 네이밍 규칙
globs: docs/**/*.md
---

# 문서 네이밍 규칙

## 필수 ID 형식
- 기능: F-{DOMAIN}-{NNN} (예: F-USER-001)
- 화면: SCR-{NNN} (예: SCR-001)
- TC: TC-{F-ID}-{NNN} (예: TC-USER-001-001)
- ADR: ADR-{NNN}
- 에러: ERR-{DOMAIN}-{NNN}

## 파일명 규칙
- 화면설계서: docs/05_screen/screens/SCR-{NNN}.md
- ADR: docs/07_arch/adr/ADR-{NNN}.md
- 나머지: 폴더별 고정 파일명 사용 (feature-definition.md 등)

## 금지
- ID 없는 기능/화면 정의
- 폴더 규칙 벗어나는 파일 생성
