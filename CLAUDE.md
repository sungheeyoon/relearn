# Relearn — Schema & 운영 방침 (v1.0)

이 문서는 **AI(Claude)가 이 지식 저장소를 어떻게 관리하는지**를 정의하는 스키마다.
Andrej Karpathy의 "LLM Wiki" 패턴을 개인 **재학습(re-learning)** 환경에 맞춰 확장한다.

> ## 목적
> 1. 공부한 내용을 축적한다.
> 2. 미래의 내가 **빠르게 재습득**할 수 있게 한다. (목표는 기억 보존이 아니라 **재학습 비용 감소**)
> 3. AI로 **리캡(Recap)·학습 보조**를 수행한다.
> 4. 내가 가졌던 **오개념**과 **어떻게 이해했는지(과정)**를 기록한다.
> 5. 구조보다 **지속 가능성**을 우선한다.
>
> ## 역할
> - **사람(나)**: source를 수집하고 질문하고, 내 이해를 기록한다.
> - **AI(너)**: source를 읽고 객관 정리를 생성·연결·점검하고, 리캡을 진행한다.
> - `sources/`는 **읽기 전용이며 절대 수정하지 않는다.**
> - 이 저장소는 Obsidian Vault이자 **Private GitHub 저장소**다.

```
배움 → 정리 → 망각 → 리캡 → 오개념 수정 → 재습득 → 지식 성장
```

---

## 0. 핵심 원칙 (다른 모든 규칙에 우선)

1. **완벽함보다 지속성** — 완벽한 노트보다 계속 쓰는 노트가 낫다. 구조 때문에 기록을 포기하지 않는다.
2. **AI는 보조자** — 정리·연결을 돕는다. 최종 판단과 이해는 사용자의 몫이다.
3. **사람은 원래 망각한다** — 망각을 전제하고, 빠른 복구를 설계한다.
4. **30개 임계점 규칙** — 노트가 **30개 미만**이면:
   - 폴더 구조를 늘리지 않는다 · 태그 체계를 확장하지 않는다 · MOC를 세분화하지 않는다.
   - **기록과 리캡 자체**에 집중한다.
   - 30개 이상 누적 후, 실제 사용 경험을 기반으로 구조를 재평가한다(폴더 재구성 등).

> 좋은 시스템의 기준: ① 3개월 뒤에도 쓰고 있는가 ② 망각 후 빠르게 복구되는가 ③ 기록이 부담스럽지 않은가.

---

## 1. 폴더 구조

```
sources/          raw source (불변, 사람이 추가, AI는 읽기 전용)
notes/concepts/   개념 — 아이디어, 이론, 방법, 패턴
notes/entities/   고유명사 — 사람, 도구, 조직, 제품, 논문
notes/maps/       MOC(Map of Contents) — 허브 역할만. Dataview로 목록 자동생성.
schema/           템플릿·규칙 (concept-template.md 등)
inbox/            빠른 캡처. ingest 때 비워진다.
00-index.md       전체 홈 / 대시보드. 모든 map으로 가는 출발점.
```

각 타입 폴더 안은 **평탄(flat)**하게 유지한다(하위 폴더 중첩 금지). 그룹핑은 `topics`/태그/MOC로.
> 30개 임계 전까지 이 구조를 **고정**한다. `wiki/`·`index/` 등으로의 폴더 재명명은 임계 후 재평가 대상.

### 1.1 `sources/` vs `notes/concepts/` — 경계 (혼동 금지)

이 둘을 섞으면 "AI가 자기 산출물을 원본인 척 재인용"하는 순환이 생겨 출처 추적이 무의미해진다. 아래 경계를 지킨다.

- **`sources/` = 학습 재료(원본).** concept를 만드는 입력. body는 불변 — AI는 raw 본문을 **수정하지 않는다.**
  (단, provenance 메타데이터인 frontmatter는 additive로 보정 가능. 사실 삭제·본문 변경은 금지.)
- **`notes/concepts/` = 소스를 소화한 객관 정리.** AI가 생성·연결·갱신하는 대상.
- 흐름은 **단방향**: `sources/` → `concepts/`. concept를 다시 source로 승격하지 않는다.

**소스 유형 — 모든 소스는 frontmatter `origin` 필수:**

| `origin` | 뜻 | 예 |
|---|---|---|
| `external` | 외부에서 온 원본 (웹·책·논문·gist) | karpathy-llm-wiki |
| `user-note` | 사용자가 직접 작성·정리한 학습 노트 | 직접 쓴 공부 메모 |
| `ai-session` | AI(Claude)와의 학습 세션 기록 | 학습 대화 세션 정리본 |

> `user-note`/`ai-session`도 "그 시점의 학습 재료"로서 `sources/`에 둔다(30개 임계 전 새 폴더 금지, §0-4).
> 단 `origin`으로 정체를 반드시 구분해, 무엇이 검증된 외부 사실이고 무엇이 세션 산출물인지 드러낸다.

**concept 출처 추적 규칙:**
- 소스 파일에서 나온 concept → frontmatter `sources: ["[[..]]"]`로 연결.
- 대화 세션에서 즉석으로 만든 concept(대응 소스 파일 없음) → `sources: []`를 두되,
  본문 H1 아래 `> 💭 AI 정리: 대화 세션 기반, 외부 raw source 없음.` 을 명시한다.
  → "출처 전무"를 금지하고 "출처=대화로 명시"로 만든다(§4 추적성 준수).

---

## 2. 파일명 규칙 (kebab-case)

- 소문자만, 단어는 하이픈(`-`)으로, 공백 금지.
- 금지 특수문자: `[ ] # ^ | : ? * \ / " < >`
- 예: `llm-wiki-pattern.md`, `andrej-karpathy.md`

> 새 파일 생성 전 **기존 노트를 먼저 검색**해 같은 개념이 있으면 새로 만들지 말고 갱신한다.

## 3. aliases 정책

기계 친화성은 파일명(kebab)으로, 인간 친화성·검색은 `aliases`로 확보한다. 모든 노트가 포함:
1. 영어 Title Case 별칭  2. 자연스러운 한국어 번역명  3. 널리 쓰이는 약어(있으면)

```yaml
aliases: [Retrieval Augmented Generation, 검색 증강 생성, RAG]
```
> 한/영 병용 vault에서 aliases는 검색의 생명줄이다. 비우지 않는다.

---

## 4. Frontmatter 표준

모든 노트는 아래 순서를 따른다. (concept 기준; entity/map은 `understanding`·`My Process`를 생략)

`sources/` 파일도 frontmatter를 **반드시** 가진다(§1.1). 소스 표준:

```yaml
---
title:                  # 사람이 읽을 제목 (따옴표 가능)
type: source            # 항상 source
origin:                 # external | user-note | ai-session  (필수)
tags:                   # [source, ...]
created:                # YYYY-MM-DD
author:                 # 저자/작성 주체
url:                    # 있으면 (external 권장, 없으면 빈값)
---
```

concept/entity/map 표준:

```yaml
---
title:                  # kebab-case 파일명
aliases:                # 영문 / 한국어 / 약어
type:                   # concept | entity | map
status:                 # seed | growing | mature | stub
understanding:          # confused | clicked | teachable   (concept만)
created:                # YYYY-MM-DD
updated:                # YYYY-MM-DD
last_recap:             # 마지막 리캡 날짜 (없으면 빈값)
sources:                # provenance [[..]]
topics:                 # MOC 연결용 (map 파일명)
extends:                # 관계 — 아래 5절 참조 (wikilink 리스트)
contradicts:
contrasts:
examples:
tags:
---
```

- `status`(노트 성숙도)와 `understanding`(내 이해도)은 **별개의 축**이다. 혼동하지 않는다.
- 출처 추적: 모든 사실은 `sources` 또는 본문 `(출처: [[..]])`로 추적 가능해야 한다. 대응 소스가 없는 대화-생성 concept는 §1.1의 `> 💭 대화 세션 기반` 표기로 추적성을 확보한다(`sources: []` 방치 금지).
- AI의 추론은 `> 💭 추론:` 으로 표시한다.
- 신규 concept는 `schema/concept-template.md`를 복제해 시작한다.

---

## 5. 관계 표현 규칙

**모든 관계는 frontmatter에서만 관리한다. 본문 inline field(`::`)는 사용하지 않는다.** (이원화 drift 방지)
값은 그래프 뷰가 인식하도록 **`[[wikilink]]` 리스트**로 적는다.

```yaml
extends:    ["[[zettelkasten]]"]          # 계승/확장하는 상위 개념
contradicts:["[[..]]"]                    # 모순 (지우지 말고 연결만)
contrasts:  ["[[rag-vs-context-wiki]]"]   # 대조
examples:   ["[[..]]"]                     # 사례/구현체
```

- **모순은 지우지 않는다.** 양쪽 노트의 `contradicts`로 연결하고 쟁점으로 남긴다(수렴 강요 금지).
- 타입드 관계가 애매하면 `contrasts`로 둔다.

---

## 6. status / understanding 정책

**status (노트 성숙도)**
- `seed` 새로 생성, 링크 적음 · `growing` 반복 수정·여러 source 연결 · `mature` 성숙·여러 곳 참조·lint 통과 · `stub` 자리만 잡은 미작성 노트.

**understanding (내 이해도, concept만)**
- `confused` 아직 안 잡힘.
- `clicked` 정의·왜 필요·무슨 문제 해결, 이 3개를 설명할 수 있음.
- `teachable` 아래 **이해 완료 7-게이트**를 전부 통과 (익숙함 ≠ 이해):
  ① 정의 · ② 왜 필요 · ③ 무슨 문제 해결 · ④ 언제 사용 · ⑤ 언제 쓰면 안 되나(반례) · ⑥ 직접 예제 제작 · ⑦ 남에게 쉽게 설명.
  → 하나라도 막히면 아직 teachable 아님.
- AI는 **리캡 결과로만** 변경을 *제안*하고, 사용자 동의 후 갱신한다. (임의 변경 금지)

---

## 7. 노트 생성 규칙

새 개념을 배웠을 때: ① source 저장 → ② AI 객관 정리 → ③ (필요시) 내 이해 과정 기록 → ④ 관계 연결 → ⑤ 리캡 대상화.

**원자성 4-Test** (3개 이상 충족 시 독립 노트, 미달 시 기존 노트의 섹션으로):
1. 독립 설명 가능? 2. 반복 참조 가능성? 3. 향후 source 추가 가능성? 4. MOC 포함 가치?
> "links earn notes": 처음엔 섹션/언급, 두 번째 참조에서 독립 노트로 승격.
> 단, **중요하지만 아직 못 쓴 개념은 `status: stub`로 자리만 잡아도 된다**(깨진 링크 방치보다 stub이 낫다).

---

## 8. 핵심 학습 워크플로우

### Ingest
새 source 읽기 → atomic 노트 추출·갱신(4-Test) → `topics`·관계 연결 → inbox 비우기 → 보고.

### Query
위키 기준으로 답하고 근거를 `[[링크]]`로 제시 → 가치 있으면 노트화 제안.

### Recap Loop  — 트리거: "리캡해줘" / "복습하자" / "퀴즈 내줘"
**대상 선정**: 특정 노트 지정이 없으면 `last_recap`이 **오래됐거나 비어 있는** 노트부터 고른다(간격 반복). 사람용 자동 뷰는 [[recap-dashboard]](Dataview). AI는 Dataview를 실행 못 하므로 raw 노트의 `last_recap`을 **직접 스캔**해 오래된 순으로 정한다.
1. `## 🧠 My Process & Understanding`를 **제외한** 본문을 우선 파싱한다.
2. 핵심 개념을 1~2줄로 환기시킨다.
3. **Feynman 기반 질문 1개**를 생성해 사용자가 스스로 설명하게 유도한다.
4. 답변을 평가한다.
5. `understanding` 변경을 **제안**한다 → 사용자 동의 후 갱신하고 `last_recap`을 오늘 날짜로 기록.

**간격 스케줄 (spaced repetition)** — 장기기억은 *간격 반복 + 인출 연습(Feynman)*으로 만든다. 한 세션에 몰아 올리지 않는다:
- `confused` → **2~3일** 안에 재학습(우선 재이해) · `clicked` → **3일 → 7일 → 14일**로 간격 확장 · `teachable` → **30일**마다 유지보수 복습.
- `last_recap`이 비면 = **인출 미검증** → 최우선. 통과 시 `last_recap`을 그날로 갱신해 다음 기한을 뒤로 민다.

> 사용자의 "이해했다 / 이해한 것 같다"는 자기보고만으로 `understanding`을 올리지 않는다.
> 반드시 설명·적용·예시·반례 질문으로 검증한 뒤 판정한다.
> 특히 `teachable` 승격은 6절의 7-게이트 중 미검증 항목을 콕 집어 질문한다.

### Deconstruct Loop  — 트리거: "이거 이해 안 돼" / "무슨 말인지 모르겠어" / "차이가 뭐야"
사전적 정의 나열을 우선하지 않는다. **반드시 기존 wiki를 탐색해 대조 학습**으로, 순서대로:
1. 🤝 무엇이 같은가? 2. ⚖️ 무엇이 다른가? 3. 💡 왜 이 개념이 등장했는가? 4. 🔗 내 지식 지도 어디에 연결되는가?

### Lint (Dataview 기반)
고아(inlinks=0) · 깨진 링크 · frontmatter 누락 · 미연결 모순 · seed/stub 정체(>30일) 점검 → 보고(자동 수정은 승인 후).
**경계 점검(§1.1)**: 소스에 `origin` 누락 · `sources: []`인데 대화-출처 표기도 없는 concept · concept가 source로 역류했는지도 함께 본다.

---

## 9. My Process & Understanding 가드레일

각 concept 노트의 `## 🧠 My Process & Understanding` 섹션은 **사용자 주관 영역**이다.
- AI는 사용자의 **명시적 요청 없이 이 섹션을 수정·삭제하지 않는다.**
- 사용자 발화/Inbox 메모를 **의미 왜곡 없이 정제**하는 것만 허용.
- 선택 사항이며 비어 있어도 된다. entity/map 노트는 보통 비운다.

---

## 10. Dataview 기반 MOC

MOC는 수동 링크 목록을 유지하지 않는다. 노트가 `topics`로 MOC에 연결되고, MOC는 Dataview로 자동 생성.
> 대시보드/MOC는 **사람 눈을 위한 것**이다. AI는 Dataview를 실행하지 못하므로 **raw 파일을 직접 탐색**해 답한다.

````markdown
```dataview
TABLE status, understanding, updated
FROM "notes"
WHERE contains(topics, this.file.name)
SORT updated DESC
```
````

---

## 11. AI 권한 모델

### Additive — 자동 수행 가능
위키 링크/aliases/frontmatter/`topics`/관계/status 갱신 · Dataview 필드 추가 · lint 제안 ·
커밋 메시지 생성 · 새 노트·stub 생성(7절) · Recap/Deconstruct 진행.

### Destructive — 사용자 승인 필요
노트 삭제·병합·분리·이름 변경 · 폴더 재구성 · `CLAUDE.md` 변경 · **`My Process` 섹션 수정** ·
출처 기반 사실의 *덮어쓰기/반박* · 모순의 *해소(판정)* · `understanding` 임의 변경 ·
`git revert`/`reset` · 브랜치 삭제 · 강제 푸시 · **`git push`**.

> AI는 출처 기반 사실을 삭제하지 않는다(추가·주석만). 모순은 표시만 한다.

---

## 12. Git & 커밋 규칙

- **`git push`는 항상 사용자의 명시적 요청 후에만.** 자동 push 금지.
- `.claude/settings.local.json`의 allow 목록에 **`Bash(git push *)`가 존재해서는 안 된다.** 문서 정책과 실제 권한 설정은 반드시 일치한다.
- AI는 커밋 메시지를 **제안**하고 승인 후 커밋. 단일 `main`, 선형 히스토리. **소스 1개 = 커밋 1개.**

### 커밋 메시지 컨벤션 (지식 vault 전용)

형식: **`type(scope): 요약`**
- **요약은 한글로** 쓴다. 현재형, 마침표 없음, ~50자.
- **scope** = 대상 노트(kebab 파일명) 또는 영역(`v1.0`·`home`·`orphans` 등).
- 본문(선택): 바뀐 노트·핵심 변경을 bullet로.

| type | 언제 |
|---|---|
| `ingest` | 새 source 적재 → 노트 생성/갱신 |
| `note`   | 소스 없이 노트 본문 직접 보강 |
| `recap`  | 리캡 결과(understanding·last_recap·My Process) 기록 |
| `link`   | 관계/위키링크/topics 연결 변경 |
| `lint`   | 고아·깨진 링크·모순 정리 |
| `schema` | CLAUDE.md·템플릿·규칙 변경 |
| `index`  | MOC·대시보드·홈(00-index) |
| `chore`  | 설정·gitignore 등 |

예시:
- `ingest(react): composition·children 학습 노트 적재`
- `recap(llm-wiki-pattern): clicked 승급`
- `schema(v1.0): 이해도 축 도입`

`recap` 커밋은 이해도 변화를 본문 끝에 트레일러로 남긴다(선택):

```
Understanding: confused → clicked
```

---

## 13. 확장

vault가 ~50–100k 토큰(약 150–200페이지)을 넘으면 전체-context 위키의 한계 → MOC 단위 부분 로딩으로 전환을 고려.
그 전, **30개 임계**에서 폴더/태그/MOC 구조를 1차 재평가한다(0절 4번).

---

## 14. 매일의 시스템 — 코치 역할 (행동층) — 선택 모듈

> **이 절은 옵트인이다.** 지식층(§1~13)은 이 절 없이도 완결적으로 작동한다.
> 습관 코칭이 필요 없으면 §14·`schema/daily-system.md`·`log.md`를 무시하거나 삭제해도 된다.

지식층(§1~13)은 공부한 날에만 작동한다. 이 절은 "공부하는 날"을 만드는 행동층의 AI 배선이다.
상세 규칙·마음가짐은 `schema/daily-system.md`, 일일 장부는 루트 `log.md`.

**대원칙: AI는 사용자를 평가하지 않는다.** 공부량 지적·공백 일수 나열·"더 하면 좋다" 류 발화 금지.
바닥을 쳤으면 그날은 성공이다 — 이것이 사용자와의 사전 계약이다.

### 세션 시작 — 트리거: "오늘" (또는 리캡/학습 세션의 첫 응답)
`log.md`와 노트들의 `last_recap`을 스캔해 **"오늘의 바닥" 한 줄만** 제시한다.
(예: "오늘의 바닥: [[zettelkasten]] 리캡 문답 1개.") 백엔드 계산 과정·밀린 목록은 보여주지 않는다.

### 공부 지표 (편법 불가 — 흔적이 남는 행동만 인정)
- **바닥** = 리캡 문답 1개 **또는** inbox 캡처 1줄
- **표준** = 리캡 3문답 이상 **또는** ingest 1세션(소스+노트 생성)
- **천장** = 표준의 2배 지점 → AI가 먼저 "여기서 끊자"고 제안하고, 남은 궁금증을 inbox에 한 줄로 남긴다.

### log.md 기록 (Additive — 자동 수행)
- **공부 줄**: 세션의 학습 활동이 끝나면 AI가 그날의 실제 산출물(문답 수·노트·커밋)을 근거로 단계를 판정해 append. 형식: `7/13 공부:표준(리캡3) 운동:바닥`
- **운동 줄**: 사용자가 "운동 표준"/"어제 운동 X"라고 말하면 해당 날짜 줄에 append/갱신.
- 같은 날 줄이 이미 있으면 새 줄을 만들지 않고 그 줄을 갱신한다.

### 판정 외주 — 트리거: "주간 리뷰" / "래칫" (또는 마지막 리뷰 후 2주 경과 시 AI가 먼저 제안)
`log.md`를 집계해 바닥 유지율·주간 표준 달성률을 보고하고, 래칫 판정(2주 90% 이상 → 한 칸 상향 / 상향 후 붕괴 → 즉시 하향)을 **제안**한다. 사용자는 승인/거부만. 결과는 `schema/daily-system.md`의 표를 갱신해 기록.

### 재시작 프로토콜
`log.md`에 공백(빈 날들)이 보여도 언급을 최소화한다. 리셋·정산·만회 없음 — "오늘의 바닥" 제시로 끝.

### 리캡과의 연결
- `understanding` 승급은 §6·§8 그대로: 사용자의 자기보고("이건 장기기억 간 것 같아")로 올리지 않는다. 그 발화는 **리캡 큐 등록 신호**로 받아 inbox에 리마인더를 남기고, 다음 리캡에서 검증 후 승급한다.
- 학습 세션 종료 시 문서화(소스 저장 → concept 생성 → 커밋 제안)는 §8 워크플로우 그대로 진행하고, 마지막에 log.md 기록까지가 세션 마감이다.
