---
title: "Karpathy — LLM Wiki 패턴"
type: source
origin: external
tags: [source, llm, knowledge-management]
created: 2026-06-15
author: Andrej Karpathy
url: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
---

# Karpathy — LLM Wiki 패턴 (raw source)

> 출처: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
> 이 파일은 raw source다. AI는 수정하지 않는다.

## 핵심 개념
전통적 RAG(매번 원문에서 답을 재생성)대신, LLM이 **점진적으로 업데이트하는
영속적·상호연결된 마크다운 지식베이스(LLM Wiki)**를 유지한다.

## 3계층 구조
- **Raw Sources** — 불변의 문서 모음. LLM은 읽기만.
- **The Wiki** — LLM이 만든 마크다운(엔티티/개념/요약/개요). 상호참조와 일관성을 LLM이 관리.
- **The Schema (예: CLAUDE.md)** — 구조·규칙·워크플로우(ingest/query/maintenance)를 정의.

## 핵심 동작
- **Ingest**: 새 소스 처리 → 정보 추출, 엔티티/개념 페이지 갱신, 인덱스 유지
- **Query**: 위키에 질문 → 답을 새 페이지로 파일링 가능
- **Lint**: 모순·오래된 주장·고아 페이지·누락 상호참조 점검

## 왜 작동하나
지식베이스 유지의 고된 부분은 읽기/생각이 아니라 bookkeeping(상호참조 갱신 등).
LLM은 지루해하지 않고 잊지 않는다. 사람은 소스를 큐레이션하고 질문, LLM은 문서 유지를 담당.

## 중요한 구분
~50–100k 토큰(약 150–200페이지) 이하 규모에선 context 기반 위키가 RAG보다 우수:
100% 검색 신뢰도, 인프라 불필요, 스니펫 짜깁기 대신 corpus 전역 추론.

## 커뮤니티에서 논의된 설계 쟁점
- 인문학의 모순: 수렴형 인식론이 부적합할 수 있음(모순이 곧 지적 내용).
- 타입 있는 관계: contradicts/extends/supersedes 엣지를 frontmatter로.
- 멀티 작성자: 파일별 append-only/commutative 쓰기가 merge 충돌에 유리.
- 할루시네이션: Socratic 대화로 위키 진입 전 가설 검증.

## 실용 도구
Obsidian Web Clipper, Marp, Dataview, Graph view, Git.
