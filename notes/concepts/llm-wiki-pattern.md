---
title: llm-wiki-pattern
aliases:
  - LLM Wiki Pattern
  - LLM 위키 패턴
type: concept
status: seed
understanding: confused
created: 2026-06-15
updated: 2026-06-15
last_recap:
sources: ["[[2026-06-15-karpathy-llm-wiki]]"]
topics: [knowledge-management]
extends: ["[[zettelkasten]]"]
contradicts: []
contrasts: []
examples: []
tags: [llm, knowledge-management]
---

# LLM Wiki 패턴

**LLM이 영속적·상호연결된 마크다운 지식베이스를 점진적으로 만들고 유지하는 방식.**
매번 원문에서 답을 재생성하는 대신, 한 번 정리한 지식을 누적·개선한다. 이 패턴은 [[andrej-karpathy]]가 "LLM Wiki"로 제시했다.

## 3계층
1. **Raw Sources** — 불변 원문. LLM은 읽기 전용.
2. **The Wiki** — LLM이 생성·관리하는 노트. 상호참조/일관성 담당.
3. **The Schema** — 구조·규칙·워크플로우 정의 (이 저장소의 `CLAUDE.md`).

## 세 가지 동작
- **Ingest** — 소스 → atomic 노트로 추출·갱신·연결.
- **Query** — 위키 기준으로 답하고, 가치 있으면 노트로 파일링.
- **Lint** — 모순·고아·깨진 링크·오래된 주장 점검.

## 핵심 통찰
지식베이스의 진짜 노동은 *읽기/생각*이 아니라 **bookkeeping**(상호참조·정리). LLM은
지루해하거나 잊지 않으므로 이 부분을 거의 공짜로 대신한다. 사람은 큐레이션과 질문에 집중.

## 관계
- [[zettelkasten]]를 계승 — 원자적 노트 + 링크라는 공통 뿌리. (관계: `extends`)

> 💭 추론: 이 저장소(Relearn) 자체가 이 패턴의 구현체다.

---

## 🧠 My Process & Understanding

> ⚠️ AI 가드레일:
> - 사용자의 명시적 요청 없이는 이 섹션을 **수정·삭제하지 않는다.**
> - Inbox 메모/사용자 발화를 **의미 왜곡 없이 정제**하는 것만 허용.
> - 사용자는 언제든 직접 수정한다.
> - 선택 사항이며 비어 있어도 된다. (entity/map 노트는 보통 비운다.)

### 내 언어로 한 줄 요약
-

### 핵심 비유 / 메타포
-

### 오개념 수정 기록 (재습득 복구 힌트)
- 처음 가졌던 생각:
- 깨달은 실제 원리:
- 왜 헷갈렸나:
