# 스킬 맵: 단계별 스킬 분류
> 최종수정: 2026-04-05 | 상태: 초안

## 전체 라이프사이클

```
[설계] → [개발] → [운영]
```

---

## A. 설계 단계 스킬 (현재 하네스)

| 스킬 | 역할 | 전제조건 |
|------|------|---------|
| requirements-preprocessor | 비정형 요구 → 구조화 | RFP/미팅노트 입력 |
| requirements-analyzer | → 정식 SRS (6개 품질기준) | preprocessor 출력 |
| doc-orchestrator | SRS → 10단계 문서 생성 | SRS 완성 |
| brainstorming | 설계 전 아이디어 검증 | 기능정의서 |
| software-architecture | 아키텍처 결정 (Clean/DDD/SOLID) | 기능분석 완료 |
| doc-sync | 문서 간 동기화 | 2개 이상 문서 존재 |

### 산출물 포맷 스킬
| 스킬 | 출력 | 용도 |
|------|------|------|
| docx | .docx | 공식 산출물 (제출용) |
| xlsx | .xlsx | 데이터 표, 추적 매트릭스 |
| pptx | .pptx | 이해관계자 발표 |
| pdf | .pdf | 최종 납품물 |

---

## B. 개발 단계 스킬 (다음 하네스)

| 스킬 | 역할 | 전제조건 | 트리거 |
|------|------|---------|--------|
| claude-code-setup | 개발환경 구성 (CLAUDE.md, rules, memory) | doc-orchestrator 완료 | 개발 시작 시 1회 |
| brainstorming | 기능별 구현 설계 검증 | feature-definition.md | 새 기능 시작 |
| test-generator | TC → 테스트코드 초안 생성 | test-definition.md | TC 작성 후 |
| test-driven-development | RED→GREEN→REFACTOR 강제 | 테스트 프레임워크 | 코딩 시 항상 |
| systematic-debugging | 4단계 근본원인 분석 | 에러 로그/스택트레이스 | 테스트 실패 시 |
| using-git-worktrees | 병렬 개발 (기능별 격리) | Git 설정 | 동시 다기능 개발 |
| doc-sync | 코드↔문서 동기화 | docs/ 존재 | API/화면 변경 시 |
| requesting-code-review | 셀프 리뷰 체크리스트 | 코드 완성 | PR 전 |
| finishing-dev-branch | 최종 검증 + 커밋 정리 | 리뷰 완료 | "완료" 선언 시 |

### 개발 루프
```
brainstorming → test-generator → TDD ←→ debugging → review → finish → doc-sync
                                  ↑ (실패 시)
```

---

## C. 운영 단계 스킬

| 스킬 | 역할 | 전제조건 | 트리거 |
|------|------|---------|--------|
| handover-orchestrator | 개발→운영 이관 | 모든 기능 ✅ | 개발 완료 시 1회 |
| maintenance-orchestrator | 버그수정/기능변경/장애대응 | handover 완료 | 운영 중 이슈 |
| performance-profiling | 성능 분석 (N+1, 메모리, 배치) | 운영 환경 | 성능 이슈 |
| doc-sync | docs-ops/ 동기화 | docs-ops/ 존재 | 운영 변경 시 |

---

## D. 커스텀 스킬 필요 (미구현)

| 우선순위 | 스킬 | 용도 | skill-creator로 생성 |
|---------|------|------|---------------------|
| P0 | 한국 RFP 파서 | HWP/PDF → 요구사항 추출 | 가능 |
| P1 | RTM 생성기 | 요구사항↔기능↔화면↔TC 매트릭스 | 가능 |
| P2 | WBS/공수산정 | 기능 복잡도 → 공수 추정 | 가능 |
