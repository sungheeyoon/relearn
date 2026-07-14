# 🧠 Relearn

[English](./README.en.md)

> 공부한 것을 모아두면 AI가 정리·연결하고, **잊어버린 뒤 다시 배우는 비용**을 줄여주는 개인 지식 시스템.

[Andrej Karpathy의 "LLM Wiki" 패턴](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)을
**재학습(re-learning)** 환경으로 확장했다. Obsidian으로 보고, Claude Code가 관리하고, Git으로 기록한다.

```
Raw Sources → AI Ingest → Atomic Notes → Maps(MOC) → Recap Loop → Long-term Knowledge
```

## 왜 만들었나

1. **기억 보존이 아니라 재학습 비용 감소** — 사람은 원래 망각한다. 목표는 "안 잊는 것"이 아니라
   잊은 뒤 **빠르게 복구**하는 것. 내가 가졌던 오개념과 이해 과정까지 기록해 복구 힌트로 쓴다.
2. **RAG가 아니라 full-context 위키** — 개인 지식 규모(~50–100k 토큰)에서는 벡터 검색보다
   AI가 위키 전체를 읽고 답하는 쪽이 정확하다. 인프라도 필요 없다.
3. **AI는 사서(司書), 판단은 사람** — 지식베이스의 진짜 노동은 생각이 아니라 **bookkeeping**
   (상호참조·정리·점검)이다. 지루해하지도 잊지도 않는 AI에게 그걸 맡기고, 사람은 큐레이션과 질문에 집중한다.

여기에 두 가지를 더 얹었다:

- **리캡 루프** — 간격 반복(spaced repetition) + Feynman 인출 연습. AI가 복습 기한이 지난 노트를 골라
  질문을 던지고, 답변을 검증한 뒤에만 이해도(`confused → clicked → teachable`)를 올린다. 자기보고로는 안 올라간다.
- **행동층 (선택)** — "공부하는 날" 자체를 만드는 최소 습관 시스템(바닥/표준/천장 + 래칫). 안 써도 된다.

## 시작하기

1. **[Use this template]** 버튼으로 내 저장소 생성 (private 추천 — 개인 학습 기록이 쌓이는 곳이다)
2. 클론 후 Obsidian에서 **"Open folder as vault"** 로 열기 → 커뮤니티 플러그인 **Dataview** 활성화
3. 터미널에서 이 폴더를 열고 **Claude Code** 실행 — 규칙(`CLAUDE.md`)은 자동으로 로드된다
4. 공부한 글/메모를 `sources/`에 넣고 (빠른 캡처는 `inbox/`에 한 줄) → **"ingest 해줘"**

예제 노트 2개(`llm-wiki-pattern`, `zettelkasten`)가 들어 있다 — 이 시스템 자체를 설명하는 노트라서
첫 리캡 재료로 쓰면 시스템을 배우면서 시스템 사용법도 익히게 된다.

## 이렇게 쓴다

각 워크플로우는 **Claude Code 스킬**(`.claude/skills/`)로 분리돼 있다 — `/ingest`, `/recap`,
`/deconstruct`, `/lint`, `/today`로 직접 부르거나, 아래처럼 자연어로 말하면 알아서 발동한다.
호출될 때만 로드되므로 평소 세션 컨텍스트는 가볍다.

**Ingest — 소스를 노트로**
> 나: `sources/`에 오늘 공부한 글 넣었어. ingest 해줘.
> AI: 읽고 atomic 노트 2개를 만들었어. `closure`는 기존 노트에 섹션으로 합쳤고(원자성 4-Test 미달),
> `topics: [javascript]`로 맵에 연결했어. 커밋 메시지 제안: `ingest(closure): 클로저·스코프 학습 노트 적재`

**Recap — 복습 (핵심 루프)**
> 나: 리캡하자.
> AI: `last_recap`이 가장 오래된 노트는 [[zettelkasten]] (12일 경과). 질문: 제텔카스텐에서
> "링크에 맥락이 필요하다"는 게 왜 중요했지? 위키피디아 링크와 뭐가 달라?
> 나: (내 말로 설명)
> AI: 핵심은 맞아. 다만 '개별 생각 단위 링크'를 빠뜨렸어. `clicked` 유지를 제안할게 — 동의하면 `last_recap` 갱신.

**Query — 내 노트 기준으로 답하기**
> 나: 내 노트 기준으로, 제텔카스텐과 LLM 위키의 결정적 차이는 뭐였지?
> AI: [[zettelkasten]]과 [[llm-wiki-pattern]]에 따르면 차이는 "연결을 누가 잇는가" — … (근거를 링크로 제시)

**Lint — 위생 점검**
> 나: lint 해줘.
> AI: 고아 노트 1개, 깨진 링크 2개, `origin` 누락 소스 1개. 수정안을 보여줄게 — 승인하면 적용한다.

## 구조

| 경로 | 내용 |
|------|------|
| `sources/` | raw 학습 재료 (사람이 추가, AI는 **읽기 전용**) |
| `notes/concepts/` | 개념·이론·방법·패턴 — AI가 생성·연결하는 객관 정리 |
| `notes/entities/` | 사람·도구·조직·논문 |
| `notes/maps/` | MOC(Map of Contents) 허브 — Dataview로 목록 자동 생성 |
| `inbox/` | 빠른 캡처 (ingest 때 비워짐) |
| `schema/` | 노트 템플릿 · 행동층 규칙 |
| `00-index.md` | 홈 대시보드 |
| `CLAUDE.md` | **시스템의 심장** — 스키마·권한 모델 (매 세션 로드) |
| `.claude/skills/` | 워크플로우 스킬 (`/ingest` `/recap` `/deconstruct` `/lint` `/today`) — 호출 시에만 로드 |

## 설계 원칙 (요약)

- **완벽함보다 지속성** — 노트 30개 미만이면 구조를 늘리지 않는다. 기록과 리캡에만 집중.
- **객관과 주관의 분리** — AI는 소스 기반 객관 정리를 쓰고, 노트의 `🧠 My Process & Understanding`
  섹션은 사용자 영역이라 AI가 임의로 건드리지 않는다.
- **모순은 지우지 않는다** — `contradicts`로 연결하고 쟁점으로 남긴다.
- **출처 추적** — 모든 사실은 `sources` 또는 `(출처: [[..]])`로 추적 가능해야 한다.
- **권한 모델** — 추가(링크·frontmatter·새 노트)는 AI가 자동, 파괴(삭제·병합·rename·push)는 승인 필요.
  **자동 push 금지.**

전체 규칙은 [`CLAUDE.md`](./CLAUDE.md)에 있다. 이 파일이 곧 시스템이다 — 마음에 안 드는 규칙은 고쳐 쓰면 된다.

## 선택 모듈: 매일의 시스템

지식층은 공부한 날에만 작동한다. [`schema/daily-system.md`](./schema/daily-system.md)는 "공부하는 날"을
만드는 행동층이다 — 하루의 최소 단위(바닥), 진짜 목표(표준), 과열 방지(천장), 그리고 2주 데이터로만
난이도를 조정하는 래칫 규칙. AI는 코치 역할을 하되 **절대 사용자를 평가하지 않는다.**
필요 없으면 `CLAUDE.md` §14, `schema/daily-system.md`, `log.md`를 삭제하면 된다.

## 크레딧 & 라이선스

- 원 아이디어: [Andrej Karpathy — LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- 방법론 뿌리: [Zettelkasten](https://zettelkasten.de/introduction/) (Niklas Luhmann)
- [MIT License](./LICENSE)
