---
name: lint
description: vault 위생 점검 - 고아 노트, 깨진 링크, frontmatter 누락, 미연결 모순, 소스 경계 위반. 트리거 - "lint 해줘", "점검해줘"
---

# Lint — vault 위생 점검

CLAUDE.md §4(frontmatter 표준), §5(관계 규칙), §1.1(소스 경계), §11(권한)을 전제한다.

## 점검 항목

- **고아 노트** — inlinks 0 (어떤 노트/MOC에서도 참조되지 않음)
- **깨진 링크** — 대상 없는 `[[wikilink]]` (방치보다 `status: stub` 생성 제안이 낫다, §7)
- **frontmatter 누락/불량** — §4 표준 필드 누락, `aliases` 비어 있음, `topics` 없음
- **미연결 모순** — 서로 어긋나는 주장인데 양쪽 `contradicts`로 연결 안 됨
- **정체 노트** — `seed`/`stub` 상태로 30일 초과
- **경계 점검(§1.1)** — 소스 `origin` 누락 · `sources: []`인데 대화-출처 표기(`> 💭 AI 정리:`)도 없는
  concept · concept가 source로 역류했는지

## 절차

1. `notes/`·`sources/` 전체를 스캔해 항목별로 수집한다.
2. 발견 사항과 수정안을 보고한다.
3. **자동 수정은 승인 후에만**(§11). 승인되면 적용하고 커밋 제안: `lint(scope): 요약`(§12).

## 원칙

- 모순은 해소(판정)하지 않는다 — 양쪽을 `contradicts`로 연결·표시만 한다(§5·§11).
- 출처 기반 사실은 삭제하지 않는다 — 추가·주석만(§11).
