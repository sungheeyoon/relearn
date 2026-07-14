---
name: ingest
description: sources/나 inbox/에 새 학습 재료가 들어왔을 때 atomic 노트로 추출·갱신·연결하고 inbox를 비운다. 트리거 - "ingest 해줘", 소스 추가 후 정리 요청
---

# Ingest — 소스 → atomic 노트

CLAUDE.md의 스키마를 전제한다: §1 폴더·경계, §2 파일명, §3 aliases, §4 frontmatter, §5 관계, §7 생성 규칙, §11 권한, §12 커밋.

## 절차

1. **읽기** — `sources/`의 새 파일과 `inbox/`의 캡처를 읽는다. `sources/` 본문은 절대 수정하지 않는다(§1.1).
   frontmatter가 없거나 불완전하면 소스 표준(§4)으로 additive 보정 — `origin`(external | user-note | ai-session) 필수.
2. **기존 노트 검색** — 개념 후보마다 새로 만들기 전에 vault를 먼저 검색한다. 같은 개념이 있으면 새 파일 대신 갱신한다(§2).
3. **원자성 4-Test(§7)** — ① 독립 설명 가능? ② 반복 참조 가능성? ③ 향후 source 추가 가능성? ④ MOC 포함 가치?
   → 3개 이상이면 `schema/concept-template.md` 복제로 독립 노트, 미달이면 기존 노트의 섹션으로.
   "links earn notes" — 애매하면 섹션으로 시작하고 두 번째 참조에서 승격. 중요하지만 아직 못 쓴 개념은 `status: stub`.
4. **연결** — frontmatter에서만: `sources`(provenance) · `topics`(MOC) · `extends`/`contradicts`/`contrasts`/`examples`(§5).
   aliases 3종(영문 Title Case / 한국어 / 약어)을 채운다(§3).
5. **inbox 비우기** — 처리한 캡처는 노트에 반영한 뒤 삭제한다(ingest의 정의된 일부, §1).
6. **보고·커밋 제안** — 생성/갱신 노트, 연결, 섹션-병합 판정 근거를 보고하고 커밋 메시지를 제안한다:
   `ingest(scope): 요약`(§12). **소스 1개 = 커밋 1개.** 승인 후 커밋. push 금지.

## 주의

- 새 concept 초기값: `status: seed`, `understanding: confused`, `last_recap:` 비움.
- 대응 소스 파일 없이 대화에서 만든 concept → `sources: []` + 본문 H1 아래
  `> 💭 AI 정리: 대화 세션 기반, 외부 raw source 없음.` (§1.1 — "출처 전무" 금지)
- concept를 source로 승격하지 않는다 — 흐름은 단방향 `sources/ → concepts/`(§1.1).
