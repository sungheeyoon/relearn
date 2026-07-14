---
title: knowledge-management
aliases:
  - Knowledge Management
  - 지식 관리
  - PKM
type: map
status: growing
created: 2026-06-15
updated: 2026-06-16
sources: []
topics: []
tags: [moc, pkm, knowledge-management]
---

# 지식 관리 (Map)

노트테이킹, 제2의 뇌, AI 기반 지식 그래프에 관한 허브. 아래 목록은 각 노트의 `topics` 필드로
자동 수집된다 (Dataview 플러그인 필요).

## Related Notes

```dataview
TABLE type, status, understanding, updated
FROM "notes"
WHERE contains(topics, this.file.name)
SORT updated DESC
```

## 열린 질문
- 인문학에서 "모순"을 어떻게 보존하며 다룰까? (수렴 vs 발산 인식론)
