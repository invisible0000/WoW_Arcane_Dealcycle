# 01-README-Overview.md — Arcane Mage M+ 문서 세트 개요

> 목표: **GitHub**에서 누구나 **읽자마자 실행 가능한** 비전 법사(Mage, Arcane) **딜사이클 레퍼런스**를 제공한다.  
> 원칙: 모든 문서는 **배경·구성목적**, **관계형 표(우선순위/분기)**, **Mermaid 플로우 차트**를 **필수**로 포함한다.  
> 표/플로우의 **모든 스킬 표기**는 `한글 스킬명 (영문명) | 성격 | 시전시간` 형식을 따른다.

---

## 1) 배경 · 구성 목적

- **왜**: M+에서의 비전 법사는 **쿨다운 창(Arcane Surge/Touch of the Magi)**, **자원(Arcane Charges)**, **버프(Nether Precision, Intuition, Harmony 등)** 와 **투사체 여행시간** 상호작용으로 **분기 많은 우선순위 시스템**을 가진다. 전투 중 즉시 의사결정을 돕기 위해 **사람이 눈으로 스캔 가능한 관계형 표 + 흐름도**가 필요하다.
- **무엇을**: 단일/광역/특수 창(ToTM/Arcane Soul)/처형(Execute) 별로 **대표 딜사이클**을 *정형화*하고, **거짓 없는** 실행 순서를 제공한다.
- **어떻게**:  
  - **관계형 표**: 우선순위(PRI), 조건 키(COND_KEY), 사람 친화 조건(HUMAN_COND), 액션(ACTION_SKILL), 주석(NOTES).  
  - **Mermaid 플로우**: 결정 노드 `{조건?}`, 액션 노드 `[스킬 블록]`, Touch 선적중 규칙은 **점선**으로 시각화.

> 📌 **환경 가정**: WoW **Patch 11.2**, **Sunfury** 히어로 탤런트, Mythic+ 기준.  
> 📌 **검증 기준**: 로테이션의 **Touch 선-적중(Off-GCD)**, **Barrage 4충전 원칙(일부 예외 명시)**, **Aether Missiles만 풀채널** 등 핵심 불변규칙이 흐름도에 반영되어야 한다.

---

## 2) 리포 구조(권장)

```text
/docs
01-README-Overview\.md              ← (현재 문서)
02-SKILLS-Glossary.md              ← 스킬/버프 사전(정적 참조)
03-ST-Opener-and-Loop.md           ← 단일 오프너/루프
04-AoE-Opener-and-Loop.md          ← 광역 오프너/루프
05-ToTM-Window\.md                  ← 마법의 손길 창 전용 스크립트
06-Arcane-Soul-Window\.md           ← Arcane Soul 서브루프
07-Execute-Overrides.md            ← 처형(Execute) 오버라이드
08-Advanced-Tech-and-Validation.md ← 고급 테크/검증(선택)

```

---

## 3) 문서 간 참조 맵

| 파일 | 핵심 주제 | 선행 필요 | 후행 문서 |
|---|---|---|---|
| **01-README-Overview.md** | 전반 가이드/규칙 | 없음 | 모두 |
| **02-SKILLS-Glossary.md** | 스킬·버프 사전 | 01 | 03~07 |
| **03-ST-Opener-and-Loop.md** | 단일 오프너/루프 | 02 | 05, 06, 07 |
| **04-AoE-Opener-and-Loop.md** | 광역 오프너/루프 | 02 | 05, 06, 07 |
| **05-ToTM-Window.md** | ToTM 전용 스크립트 | 03/04 | 06, 07 |
| **06-Arcane-Soul-Window.md** | Arcane Soul 서브루프 | 03/04, 05 | 07 |
| **07-Execute-Overrides.md** | 처형 오버라이드 | 03/04, 05, 06 | - |
| **08-Advanced-Tech-and-Validation.md** | 고급 테크/검증 | 03~07 | - |

---

## 4) 공통 표기 규약

### 4.1 스킬 블록 표기 (모든 표/플로우에 동일 적용)

- **형식**: `한글 스킬명 (영문명) | 성격 | 시전시간`
  - 시전시간 표준값:
    - `즉시`(Instant), `즉시(Off-GCD)`(글로벌 비소모),
    - `시전 X.Ys`(Cast time),  
    - `채널 Z.Zs`(Channel time, **유지 시간은 숫자 명시**: 예 `채널 4.0s(유지)`).
  - 프리캐스트/선행시간은 `T–Xs`로 표기(예: `T–4s 환기 채널`).

#### 예시

```text

비전 탄막 (Arcane Barrage) | 스펜더 | 즉시
마법의 손길 (Touch of the Magi) | 버스트 | 즉시(Off-GCD)
시프팅 파워 (Shifting Power) | 보조/CDR | 채널 4.0s(유지)

```

### 4.2 조건 키(분기) 표기

- **대문자 스네이크 케이스**: `INTU_OR_TEMPO_EXP`, `HARM_GE_12`, `NP_EQ_0`, `CH_LT_3`, `EXEC_TRUE` 등.
- **우선순위 열**: `PRI`는 낮을수록 높은 우선순위(1이 최상).

### 4.3 관계형 표 스키마(기본)

| 열 | 의미 |
|---|---|
| `PRI` | 우선순위(정수, 1이 최상) |
| `COND_KEY` | 기계적 분기 키(축약) |
| `HUMAN_COND` | 사람이 읽는 자연어 조건 |
| `ACTION_SKILL` | 실행 스킬 블록<br>(\`스킬명(영문) \| 성격 \| 시전시간\`) |
| `NOTES` | 추가 설명/예외(Tempo 예외, Aether 풀채널 등) |

### 4.4 Mermaid 스타일 가이드

- `flowchart TD` 사용(위→아래).  
- **결정 노드**: `{조건?}` / **액션 노드**: `[스킬 블록]`.  
- **Touch 선-적중**: Barrage 노드와 Touch 노드를 **점선(`-.->`)**으로 연결, 라벨에 **“Barrage 명중 전 Off-GCD”**를 서술.  
- **루프 귀환**은 단일 노드(`(LOOP)`)로 단순화.

---

## 5) 상위 흐름(Guide Map)

```mermaid
flowchart TD
  A[01 Overview] --> B[02 Skills/Glossary]
  B --> C[03 ST: Opener/Loop]
  B --> D[04 AoE: Opener/Loop]
  C --> E[05 ToTM Window]
  D --> E
  E --> F[06 Arcane Soul]
  F --> G[07 Execute Overrides]
  C --> H[08 Advanced/Validation]
  D --> H
````

---

## 6) 불변 규칙(모든 문서가 공유)

1. **Touch of the Magi는 항상** \*\*Barrage 투사체 명중 “직전”\*\*에 **Off-GCD**로 사용한다.
2. **Barrage는 원칙적으로 4충전에서 사용**한다. (단, **Arcane Tempo 유지**를 위한 **예외**는 각 문서의 표에 *명시적으로* 표기)
3. **Aether Attunement 강화 미사일만 풀채널**, 그 외 Missiles는 **NP 부여 목적의 클리핑**이 기본.
4. **ToTM/Arcane Soul 창의 “마지막 GCD”는 Barrage**로 마감한다.
5. **AoE ToTM 창**에서는 **Magi’s Spark 충족을 위해 Blast 최소 1회**를 보장한다.
6. **오브 비행(Orb-in-air) 관찰 시** 직접 Orb 사용을 **지연**하고 **Barrage 우선**으로 충전 손실을 방지한다(고급 테크 문서에 절차 명시).

---

## 7) 정확성/검증(Contribution Checklist)

- [ ] 모든 스킬 블록이 `한글(영문) | 성격 | 시전시간` 형식을 만족한다.
- [ ] 모든 Mermaid 플로우가 **Touch 선-적중 점선**을 포함한다.
- [ ] **Intuition/Tempo 만료 손실** 방지 노드가 **최상위 우선순위**에 배치되어 있다.
- [ ] **Harmony 고중첩(≈12+) → Barrage** 규칙이 표/플로우에 존재한다.
- [ ] **Aether Missiles만 풀채널** 규칙이 명시되어 있다.
- [ ] **Tempo 유지 예외(0충전 Barrage 가능)**가 명시되어 있다.
- [ ] **ToTM/Arcane Soul 마지막 Barrage** 규칙이 명시되어 있다.
- [ ] AoE 문서에 **Blast 1회(Spark)** 규칙이 명시되어 있다.
- [ ] 예시 로그/더미 테스트 **오프너 10연속 무오류**, **Intuition 만료 손실 0회**로 자체 검증했다.

---

## 8) 다음 작성 순서(권장)

1. `02-SKILLS-Glossary.md` — 스킬/버프 ID와 설명(정적 참조).
2. `03-ST-Opener-and-Loop.md` — 단일 오프너/루프 표·플로우.
3. `04-AoE-Opener-and-Loop.md` — 광역 오프너/루프 표·플로우.
4. `05-ToTM-Window.md` — ToTM 전용 미니 스크립트.
5. `06-Arcane-Soul-Window.md` — Arcane Soul 서브루프.
6. `07-Execute-Overrides.md` — 처형 오버라이드.
7. (선택) `08-Advanced-Tech-and-Validation.md` — 테크/검증 메트릭.

---

## 9) 변경 이력(Changelog)

- **v1.0.0 (초판)**: 리포 구조/규약/지도 플로우 확정.

```text
```
