---
title: recap-dashboard
aliases:
  - Recap Dashboard
  - 리캡 대시보드
  - 복습 기한 보드
type: map
status: growing
created: 2026-07-03
updated: 2026-07-03
tags: [dashboard, recap, spaced-repetition]
---

# recap-dashboard

**복습 기한이 지난 노트를 오래된 순으로 자동 정렬해 보여주는 대시보드.**
장기기억은 **간격 반복(spaced repetition, 점점 늘어나는 간격으로 복습)** + **인출 연습(active recall, 스스로 꺼내 설명 = Feynman)** 으로 만들어진다. 이 보드는 "지금 뭘 복습해야 하나"를 눈으로 보게 해서 그 간격을 놓치지 않게 한다.

> 리캡 방법은 CLAUDE.md의 **Recap Loop**(Feynman 질문 → 평가 → understanding 제안)을 따른다. 자기보고로 이해도를 올리지 않는다.

## 📏 간격 스케줄 (이해도별)

```
confused   → 2~3일 안에 재학습 (아직 안 굳음, 우선 재이해)
clicked    → 3일 → 7일 → 14일 (성공할 때마다 간격 늘림)
teachable  → 30일마다 유지보수 복습
```

- `last_recap`이 비어 있으면 = **한 번도 인출 검증 안 됨** → 최우선.
- 리캡을 통과하면 `last_recap`을 그날로 갱신 → 다음 기한이 자동으로 뒤로 밀린다.

---

## 🔴 이해도 미평가 (한 번도 리캡 안 됨) — 최우선

```dataview
TABLE
  understanding AS "이해도",
  created AS "생성",
  (date(today) - date(created)).days AS "방치 일수"
FROM "notes/concepts"
WHERE !last_recap
SORT date(created) ASC
```

## 🟠 복습 기한 보드 (confused·clicked, 오래된 순)

```dataview
TABLE
  understanding AS "이해도",
  last_recap AS "마지막 리캡",
  (date(today) - date(last_recap)).days AS "밀린 일수"
FROM "notes/concepts"
WHERE understanding != "teachable" AND last_recap
SORT date(last_recap) ASC
LIMIT 10
```

> 기준: clicked은 **14일** 넘으면 밀린 것, confused는 **3일**만 넘어도 밀린 것으로 본다.

## 🟡 teachable 유지보수 (30일 주기)

```dataview
TABLE
  last_recap AS "마지막 리캡",
  (date(today) - date(last_recap)).days AS "경과 일수"
FROM "notes/concepts"
WHERE understanding = "teachable"
SORT date(last_recap) ASC
```

> teachable도 30일 넘게 안 건드리면 유지보수 복습(가볍게 1문제) 대상.

## 🟢 최근 완료 (안전 — 참고용)

```dataview
TABLE understanding AS "이해도", last_recap AS "마지막 리캡"
FROM "notes/concepts"
WHERE last_recap AND (date(today) - date(last_recap)).days <= 7
SORT date(last_recap) DESC
```

---

> 💭 추론: AI(Claude)는 Dataview를 실행하지 못하므로, "리캡하자"라고 하면 raw 파일의 `last_recap`을 직접 스캔해 이 보드와 같은 순서로 대상을 고른다. 이 노트는 **사람 눈(Obsidian)** 을 위한 자동 뷰다.
