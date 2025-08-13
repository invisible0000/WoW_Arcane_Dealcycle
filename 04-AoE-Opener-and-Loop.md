# 04 — AoE-Opener-and-Loop (광역 오프너 & 우선순위 루프)

> 대상: **WoW 11.2 · 비전 법사(Arcane) · Sunfury · Mythic+ · 다중 대상(AoE)**
> 목표: 다중 타깃에서 **즉시 재현 가능한** 오프너와 **거짓 없는** 우선순위 루프 제공.
> 원칙: **Touch 선-적중(Off-GCD)**, **Barrage 4충전 원칙(Tempo 예외 명시)**, **Aether Missiles만 풀채널**, **ToTM 창 내 Blast 1회(Magi’s Spark)**, **루프 마감 Barrage**.

---

## 1) 배경 · 구성 목적

- **왜**: AoE는 **오브-바라지(Orb Barrage) 프록**과 **투사체 비행시간**이 결합되어 **매 GCD가 다음 판단에 영향**. 잘못된 한 번의 Barrage/Orb가 **충전 손실**로 직결.
- **무엇을**: **대쿨 포함 ‘빅 고(Big Go)’ 오프너**, **미니 고(Mini Go)**, **일반 루프**를 **관계형 표 + Mermaid 흐름도**로 문서화.
- **핵심 차이(ST 대비)**:
  - **Explosion**을 **최대 3충전까지** 빌더로 사용.
  - **NP(네더 프리시전)**은 **Barrage 결정에 거의 영향 X**(단, **Harmony**와 결합 시 우선).
  - **ToTM 창에서 Blast 1회**는 **필수**(Magi’s Spark 충족).

---

## 2) 본 문서에서 사용하는 스킬(블록 표기 규약 포함)

> **블록 형식**: `한글 스킬명 (영문명) | 성격 | 시전시간`

| ID | 스킬 블록 | 설명/비고 |
|---:|---|---|
| S_EVO | **환기 (Evocation) \| 쿨다운/버프 시동 \| 채널 3.0s** | **T–4s 프리채널**로 버스트 준비 |
| S_SURG | **비전 폭발력 (Arcane Surge) \| 쿨다운/버스트 \| 시전 1.5s** | Big Go의 핵심 대쿨 |
| S_TOTM | **마법의 손길 (Touch of the Magi) \| 쿨다운/창구 \| 즉시(Off-GCD)** | **Barrage 명중 ‘전’**에 사용(선-적중) |
| S_BARR | **비전 탄막 (Arcane Barrage) \| 스펜더 \| 즉시** | AoE 핵심 스펜더/클리브 |
| S_ORB | **비전 구슬 (Arcane Orb) \| 빌더 \| 즉시** | **<3충전**에서 최우선 빌더, 프록 반응 |
| S_MISS | **비전 화살 (Arcane Missiles) \| 빌더 \| 채널 2.5s** | **Aether만 풀채널**, 그 외 **클립** |
| S_BLAS | **비전 작열 (Arcane Blast) \| 빌더 \| 시전 2.25s** | **ToTM 창 내 1회 필수**(Spark) |
| S_EXPL | **비전 폭발 (Arcane Explosion) \| 빌더 \| 즉시** | **0~3충전까지** 채움(근접) |
| S_SPWR | **시프팅 파워 (Shifting Power) \| 보조/CDR \| 채널 4.0s(유지)** | 버스트 직후 CDR 채널 |

> **상태 키**: `NP`(Nether Precision), `INTU`(Intuition), `TEMPO`(Arcane Tempo), `HARM`(Arcane Harmony), `AETH`(Aether Attunement), `CH`(현재 충전), `GI`(Glorious Incandescence).

---

## 3) AoE 오프너

### 3.1 Big Go(대쿨 포함) — 오프너 시퀀스 테이블

| STEP | PRI | COND_KEY | HUMAN_COND | ACTION_SKILL | NOTES |
|---:|---:|---|---|---|---|
| 1 | 1 | PRE_T_MINUS_4 | 풀 T–4s | **환기(Evocation) \| 쿨다운/버프 시동 \| 채널 3.0s** | Siphon Storm 등 사전 버프 |
| 2 | 2 | PULL_TIME_0 | 풀 0s | **비전 폭발력(Arcane Surge) \| 쿨다운/버스트 \| 시전 1.5s** | 탱커 집결/그룹 확인 후 |
| 3 | 3 | CH_LT_3_PRE | 충전 < 3 | **비전 구슬(Arcane Orb) \| 빌더 \| 즉시** | 4충전 목표(필요 시 추가) |
| 4 | 4 | CH_EQ_4_READY | 4충전 확보 | **비전 탄막(Arcane Barrage) \| 스펜더 \| 즉시** | **첫 탄막 발사** |
| 5 | 5 | TOUCH_OFFGCD | Barrage **명중 전** | **마법의 손길(ToTM) \| 쿨다운/창구 \| 즉시(Off-GCD)** | **선-적중 규칙** |
| 6 | 6 | OPENER_2ND_BARR | ToTM 진입 직후 | **비전 탄막(Barrage) \| 스펜더 \| 즉시** | 창 초반 2타 |
| 7 | 7 | AETH_CHECK | Aether 대기? | **비전 화살(Missiles) \| 빌더 \| 채널 2.5s(풀채널/클립)** | Aether면 **풀채널** |
| 8 | 8 | SPARK_BLAST_ONCE | Magi’s Spark | **비전 작열(Blast) \| 빌더 \| 시전 2.25s** | **ToTM 창 내 1회 필수** |
| 9 | 9 | BARR_FINISH_EARLY | Harmony/프록 | **비전 탄막(Barrage) \| 스펜더 \| 즉시** | 프록·하모니에 반응 |
| 10 | 10 | SHIFT_AFTER_BURST | 대쿨 직후 | **시프팅 파워(Shifting Power) \| 보조/CDR \| 채널 4.0s(유지)** | 다음 주기 당김 |
| 11 | 11 | ENTER_AOE_LOOP | 이후 | — | **AoE 루프**로 전환 |

### 3.2 Mini Go(대쿨 없음, Touch만 사용) — 요약 시퀀스

- **(필요 시) Orb로 4충전 확보 → Barrage 발사 → (비행 중) Touch(Off-GCD) → Barrage → Missiles(클립) → Blast 1회 → Barrage → 루프 진입.**

---

## 4) AoE 오프너 플로우(복붙용)

```mermaid
flowchart TD
  A(("START AoE")) --> B["환기(Evocation) | 쿨다운/버프 시동 | 채널 3.0s\n(T–4s 프리채널)"]
  B --> C["비전 폭발력(Arcane Surge) | 쿨다운/버스트 | 시전 1.5s"]
  C --> D{"현재 충전 < 3?"}
  D -- 예 --> E["비전 구슬(Arcane Orb) | 빌더 | 즉시"]
  E --> D
  D -- 아니오 --> F["비전 탄막(Arcane Barrage) | 스펜더 | 즉시"]
  F -. 투사체 비행 중 .-> G["마법의 손길(Touch of the Magi) | 쿨다운/창구 | 즉시(Off-GCD)\n<Barrage 명중 전>"]
  G --> H["비전 탄막(Arcane Barrage) | 스펜더 | 즉시"]
  H --> I{"Aether Missiles 준비?"}
  I -- 예 --> J["비전 화살(Arcane Missiles) | 빌더 | 채널 2.5s(풀채널)"]
  I -- 아니오 --> K["비전 화살(Arcane Missiles) | 빌더 | 채널 2.5s(클립)"]
  J --> L["비전 작열(Arcane Blast) | 빌더 | 시전 2.25s\n(Magi’s Spark 충족)"]
  K --> L
  L --> M["비전 탄막(Arcane Barrage) | 스펜더 | 즉시"]
  M --> N["시프팅 파워(Shifting Power) | 보조/CDR | 채널 4.0s(유지)"]
  N --> O[["AoE 우선순위 루프 진입"]]
````

---

## 5) AoE 우선순위 루프

> **해석 규칙**: **위에서부터 평가**하여 **최초로 만족하는 조건**의 액션을 즉시 실행.
> **Tempo 예외**: `TEMPO` 만료를 막기 위해 **0충전 Barrage 허용**.
> **Explosion 규칙**: **0\~3충전 구간**에서 **Explosion로 채움**(근접).
> **NP 처리**: **Barrage 결정은 NP 거의 무시**(단 **HARM(하모니) 고중첩**이면 **Barrage 우선**).
> **오브 반응**: **Orb Barrage 프록/비행 중 오브** 관측 시, 직접 오브 사용을 **지연**하고 **Barrage 연속**으로 충전 손실 방지.

### 5.1 우선순위 테이블

| PRI | COND\_KEY                  | HUMAN\_COND                                  | ACTION\_SKILL         | NOTES                    |                 |                         |
| --: | -------------------------- | -------------------------------------------- | --------------------- | ------------------------ | --------------- | ----------------------- |
|   1 | INTU\_OR\_TEMPO\_EXP       | **Intuition/Tempo**가 **다음 GCD 전 만료**         | \*\*비전 탄막(Barrage)    | 스펜더                      | 즉시\*\*          | **Tempo 예외 허용(0충전 가능)** |
|   2 | GI\_READY\_AND\_TOTM\_SOON | **Glorious Incandescence** 보유 & **ToTM ≤6s** | **보류(HOLD)**          | **Touch 창에 결합**(보류 후 사용) |                 |                         |
|   3 | HARM\_GE\_12               | **Harmony ≥\~12**                            | \*\*비전 탄막(Barrage)    | 스펜더                      | 즉시\*\*          | 고중첩 소모                  |
|   4 | CH\_LT\_3\_AND\_ORB\_OK    | **충전 < 3** & **Orb 사용 가능**                   | \*\*비전 구슬(Arcane Orb) | 빌더                       | 즉시\*\*          | 충전 복구 최우선               |
|   5 | NP\_EQ\_0\_AND\_CC\_GE\_1  | **NP=0** & **클리어캐스팅 ≥1**                     | \*\*비전 화살(Missiles)   | 빌더                       | 채널 2.5s(클립)\*\* | NP 재부여                  |
|   6 | CH\_EQ\_4\_AND\_CC\_GE\_1  | **4충전 & 클리어캐스팅 ≥1**                          | \*\*비전 탄막(Barrage)    | 스펜더                      | 즉시\*\*          | AoE 전용 규칙               |
|   7 | CH\_LE\_3                  | **충전 ≤3**                                    | \*\*비전 폭발(Explosion)  | 빌더                       | 즉시\*\*          | **최대 3충전까지** 채움         |
|   8 | CH\_EQ\_4\_AND\_ORB\_SOON  | **4충전 & Orb 재사용 임박**                         | \*\*비전 작열(Blast)      | 빌더                       | 시전 2.25s\*\*    | Spark/주기 정렬             |
|   9 | ELSE\_BLAST                | 그 외 전부                                       | \*\*비전 작열(Blast)      | 빌더                       | 시전 2.25s\*\*    | 기본 채움                   |
|  10 | OOM\_FALLBACK              | **마나 부족(OOM)**                               | \*\*비전 탄막(Barrage)    | 스펜더                      | 즉시\*\*          | 응급 회복/순환 재시동            |

> **ToTM/ASoul 마감 규칙**: 해당 창 **마지막 GCD는 Barrage**로 마감(거짓 금지 규칙).
> **AoE ToTM 창 특칙**: **창 안에서 Blast 1회**로 **Magi’s Spark** 충족(이미 했다면 생략).

### 5.2 루프 플로우(복붙용)

```mermaid
flowchart TD

  %% 스타일 정의
  classDef start fill:#1f2937,stroke:#111827,color:#fff,font-weight:bold
  classDef cond fill:#fff3cd,stroke:#d39e00,color:#111
  classDef action fill:#e7f5ff,stroke:#339af0,color:#0b7285
  classDef hold fill:#f1f3f5,stroke:#868e96,color:#343a40
  classDef warn fill:#ffe3e3,stroke:#fa5252,color:#c92a2a

  A(("AoE LOOP")):::start

  %% 1) 시간 민감/창 정렬
  A --> B
  subgraph SG1 ["시간 민감/창 정렬"]
    direction TB
    B{"Intuition/Tempo\n다음 GCD 전 만료?"}:::cond
    B -- 예 --> BARR1["비전 탄막(Barrage) | 스펜더 | 즉시\n(Tempo 예외 시 0충전 허용)"]:::action --> A
    B -- 아니오 --> C{"GI 보유 & ToTM ≤6s?"}:::cond
    C -- 예 --> HOLD["보류(HOLD)\nTouch 창 결합"]:::hold --> A
  end

  %% 2) 하모니/충전/오브 우선
  C -- 아니오 --> D
  subgraph SG2 ["하모니/충전/오브"]
    direction TB
    D{"Harmony ≥~12?"}:::cond
    D -- 예 --> BARR2["비전 탄막(Barrage) | 스펜더 | 즉시"]:::action --> A
    D -- 아니오 --> E{"충전<3 & Orb 가능?"}:::cond
    E -- 예 --> ORB["비전 구슬(Arcane Orb) | 빌더 | 즉시"]:::action --> A
  end

  %% 3) NP/클리어캐스팅 및 4충전 처리
  E -- 아니오 --> F
  subgraph SG3 ["NP/클리어캐스팅 & 4충전 처리"]
    direction TB
    F{"NP=0 & CC≥1?"}:::cond
    F -- 예 --> MISS["비전 화살(Arcane Missiles) | 빌더 | 채널 2.5s(클립)"]:::action --> A
    F -- 아니오 --> G{"4충전 & CC≥1?"}:::cond
    G -- 예 --> BARR3["비전 탄막(Barrage) | 스펜더 | 즉시"]:::action --> A
    G -- 아니오 --> H{"충전 ≤3?"}:::cond
    H -- 예 --> EXPL["비전 폭발(Arcane Explosion) | 빌더 | 즉시\n(최대 3충전까지)"]:::action --> A
    H -- 아니오 --> I{"4충전 & Orb 재사용 임박?"}:::cond
    I -- 예 --> BLASL1["비전 작열(Arcane Blast) | 빌더 | 시전 2.25s"]:::action --> J
    I -- 아니오 --> BLASL2["비전 작열(Arcane Blast) | 빌더 | 시전 2.25s"]:::action --> J
  end

  %% 4) 마나 예외 처리
  J{"마나 부족(OOM)?"}:::cond
  J -- 예 --> BARR_OOM["비전 탄막(Barrage) | 스펜더 | 즉시"]:::warn --> A
  J -- 아니오 --> A
```

---

## 6) 실행 체크리스트(풀·더미 리허설)

- [ ] **T–4s Evocation 채널**, **풀 직후 Surge**로 Big Go 준비.
- [ ] **(필요 시) Orb로 4충전 확보 → Barrage → (비행 중) Touch(Off-GCD) → Barrage** 무오류.
- [ ] **ToTM 창 안 Blast 1회**(Magi’s Spark) 포함 확인.
- [ ] **Intuition/Tempo 만료 손실 0회**.
- [ ] **Harmony ≥\~12**에서 **Barrage 지연 없음**.
- [ ] **Aether Missiles만 풀채널**, 그 외 **클립** 일관 유지.
- [ ] **Explosion은 3충전까지만** 채움.
- [ ] **Orb Barrage 프록/오브 비행** 관측 시 **직접 Orb 지연** + **Barrage 연속**으로 충전 손실 방지.
- [ ] 마나 급감 시 **응급 Barrage**로 순환 재시동.

---

## 7) 변경 이력

- **v1.0.0**: 초판 — AoE 오프너(Big/Mini), 우선순위, Spark 규칙, Orb-반응 로직 확정.

```text
::contentReference[oaicite:0]{index=0}
```
