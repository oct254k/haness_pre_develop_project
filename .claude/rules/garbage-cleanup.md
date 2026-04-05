---
description: 불필요한 파일/코드 정리 규칙
globs: "**/*"
---

# 가비지 청소 규칙

## 자동 감지 대상
- 빈 파일 (내용 없는 .md)
- doc-index.md에 등록되지 않은 docs/ 파일
- sync-status.md에서 3회 이상 "미사용" 표시된 문서
- 중복 내용이 80% 이상인 문서 쌍

## 청소 절차
1. `/sync-docs` 실행 시 가비지 후보 목록 함께 출력
2. 사용자 승인 후에만 삭제
3. 삭제 시 sync-status.md에 삭제 이력 기록

## 금지
- 사용자 승인 없는 자동 삭제
- _meta/ 파일 삭제 (doc-index, sync-status는 보호)
