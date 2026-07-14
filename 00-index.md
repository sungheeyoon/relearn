---
title: 00-index
aliases:
  - Home
  - 홈
type: map
status: seed
created: 2026-07-14
updated: 2026-07-14
sources: []
topics: []
tags: [moc, home]
---

# 🧠 Relearn — 홈

전체 지식 그래프의 출발점. 주제별 지도(Map)로 들어간다.

> ⏰ **복습 먼저**: [[recap-dashboard]] 에서 기한 지난 노트를 확인하고 "리캡하자"로 시작. (간격 반복 + Feynman 인출)

## 🗺️ Maps

```dataview
LIST aliases
FROM "notes/maps"
SORT file.name ASC
```

## 🌱 최근 업데이트

```dataview
TABLE type, status, understanding, updated
FROM "notes"
WHERE type != "map"
SORT updated DESC
LIMIT 20
```

## 📌 키울 노트 (seed · stub)

```dataview
TABLE status, updated
FROM "notes"
WHERE status = "seed" OR status = "stub"
SORT updated ASC
```

> Dataview 플러그인 설치 시 위 대시보드가 동작한다.
