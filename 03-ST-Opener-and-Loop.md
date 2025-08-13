# 03 — ST-Opener-and-Loop (단일 대상 오프너 & 우선순위 루프)

> 대상: **WoW 11.2 · 비전 법사(Arcane) · Sunfury · Mythic+**
> 목표: 단일 대상에서 **즉시 재현 가능한** 오프너와 **거짓 없는** 우선순위 루프 제공.
> 원칙: **Touch 선-적중(Off-GCD)**, **Barrage 4충전 원칙(Tempo 예외 명시)**, **Aether Missiles만 풀채널**, **루프 마감 Barrage**.---

## 1) 배경 · 구성 목적

- **왜**: 단일(ST) 구간은 **대쿨(Arcane Surge)**, **창구(Touch of the Magi)**, **충전(Arcane Charges)**, **버프(Nether Precision/Intuition/Harmony/Tempo)**가 **프레임 단위**로 상호작용함.
- **무엇을**: 전투 **T–4s**부터 **ToTM 진입/마감까지**의 오프너, 이후 **우선순위 루프**를 **관계형 표 + Mermaid 흐름도**로 문서화.
- **어떻게**: 표의 열(`PRI / COND_KEY / HUMAN_COND / ACTION_SKILL / NOTES`)과 플로우의 **결정 노드**(`{조건?}`)를 그대로 **메크로/위크오라 조건**으로 매핑 가능하게 설계.

---

## 2) 본 문서에서 사용하는 스킬(블록 표기 규약 포함)

> **블록 표기 형식**: `한글 스킬명 (영문명) | 성격 | 시전시간`

| ID | 스킬 블록 | 설명/비고 |
|---:|---|---|
| S_EVO | `환기 (Evocation) \| 쿨다운/버프 시동 \| 채널 3.0s` | 풀 **T–4s 프리채널** 권장(버스트 준비) |
| S_SURG | `비전 폭발력 (Arcane Surge) \| 쿨다운/버스트 \| 시전 1.5s` | 대쿨. 일반적으로 Touch 직전 |
| S_TOTM | `마법의 손길 (Touch of the Magi) \| 쿨다운/창구 \| 즉시(Off-GCD)` | **Barrage 투사체 명중 ‘전’**에 사용 |
| S_BARR | `비전 탄막 (Arcane Barrage) \| 스펜더 \| 즉시` | **원칙: 4충전**에서 사용(Tempo 예외) |
| S_MISS | `비전 화살 (Arcane Missiles) \| 빌더 \| 채널 2.5s` | **Aether만 풀채널**, 그 외 **클립** |
| S_ORB | `비전 구슬 (Arcane Orb) \| 빌더 \| 즉시` | 충전 빠른 보충(특히 <3일 때) |
| S_BLAS | `비전 작열 (Arcane Blast) \| 빌더 \| 시전 2.25s` | NP 소모/유지 핵심 빌더 |
| S_EXPL | `비전 폭발 (Arcane Explosion) \| 빌더 \| 즉시` | 근접 보조 빌더(충전 0~1) |
| S_SPWR | `시프팅 파워 (Shifting Power) \| 보조/CDR \| 채널 4.0s(유지)` | 대쿨 직후 CDR 채널 |

> **상태 키**: `NP`(Nether Precision), `INTU`(Intuition), `TEMPO`(Arcane Tempo), `HARM`(Arcane Harmony), `AETH`(Aether Attunement), `CH`(현재 충전).

---

## 3) 단일 오프너(프리캐스트 포함)

### 3.1 오프너 시퀀스 테이블

| STEP | PRI | COND_KEY | HUMAN_COND | ACTION_SKILL | NOTES |
|---:|---:|---|---|---|---|
| 1 | 1 | PRE_T_MINUS_4 | 풀 T–4s | `환기(Evocation) \| 쿨다운/버프 시동 \| 채널 3.0s` | 프리채널 완료 직후 이동 가능 포지션 |
| 2 | 2 | PRE_T_MINUS_1 | 풀 T–1s | `비전 화살(Arcane Missiles) \| 빌더 \| 채널 2.5s` | **클립**으로 **NP 확보**(CC 소모) |
| 3 | 3 | SURGE_NOW | 풀 0s | `비전 폭발력(Arcane Surge) \| 쿨다운/버스트 \| 시전 1.5s` | 대쿨 개시 |
| 4 | 4 | CH_LT_4 | 충전 < 4 | `비전 구슬(Arcane Orb) \| 빌더 \| 즉시` | 4충전 보장까지 반복 |
| 5 | 5 | BARR_PREHIT_TOUCH | Barrage 투사체 비행 시작 | `비전 탄막(Arcane Barrage) \| 스펜더 \| 즉시` | **비행 중** 다음 단계로 |
| 6 | 6 | TOUCH_OFFGCD | Barrage **명중 전** | `마법의 손길(ToTM) \| 쿨다운/창구 \| 즉시(Off-GCD)` | **선-적중 규칙**(필수) |
| 7 | 7 | OPENER_2ND_BARR | ToTM 진입 직후 | `비전 탄막(Barrage) \| 스펜더 \| 즉시` | 창 초반 2타 확보 |
| 8 | 8 | SHIFT_AFTER_BURST | 대쿨 직후 | `시프팅 파워(Shifting Power) \| 보조/CDR \| 채널 4.0s(유지)` | CDR로 다음 주기 당김 |
| 9 | 9 | ENTER_ST_LOOP | 이후 | — | **우선순위 루프**로 전환 |

### 3.2 오프너 플로우(복붙용)

```mermaid
flowchart TD
  A((START ST)) --> B["환기(Evocation) | 쿨다운/버프 시동 | 채널 3.0s<br>(T–4s 프리채널)"]
  B --> C["비전 화살(Arcane Missiles) | 빌더 | 채널 2.5s(클립)<br>(T–1s NP 확보)"]
  C --> D["비전 폭발력(Arcane Surge) | 쿨다운/버스트 | 시전 1.5s"]
  D --> E{"충전 < 4?"}
  E -- 예 --> F["비전 구슬(Arcane Orb) | 빌더 | 즉시"]
  F --> E
  E -- 아니오 --> G["비전 탄막(Arcane Barrage) | 스펜더 | 즉시"]
  G -. "투사체 비행 중" .-> H["마법의 손길(Touch of the Magi) | 쿨다운/창구 | 즉시(Off-GCD)<br><Barrage 명중 전>"]
  H --> I["비전 탄막(Arcane Barrage) | 스펜더 | 즉시"]
  I --> J["시프팅 파워(Shifting Power) | 보조/CDR | 채널 4.0s(유지)"]
  J --> K[["ST 우선순위 루프 진입"]]
````

---

## 4) 단일 우선순위 루프(ST Priority)

> **해석 규칙**: **위에서부터 평가**하여 **최초로 만족하는 조건**의 액션을 즉시 실행한다.
> **Tempo 예외**: `TEMPO` 만료를 막기 위해 **0충전 Barrage 허용**(표기).
> **클리핑**: **Aether Missiles만 풀채널**, 그 외 Missiles은 **NP 재부여 목적**으로 **클립**.

### 4.1 우선순위 테이블

| PRI | COND\_KEY                    | HUMAN\_COND                          | ACTION\_SKILL               | NOTES |
| --: | ---------------------------- | ------------------------------------ | --------------------------- | ----- |
|   1 | INTU\_OR\_TEMPO\_EXP         | **Intuition/Tempo**가 **다음 GCD 전 만료** | `비전 탄막(Barrage) \| 스펜더 \| 즉시` | **Tempo 예외 허용(0충전 가능)** |
|   2 | HARM\_GE\_12\_AND\_NP\_LE\_1 | **Harmony ≥\~12** & **NP ≤ 1**       | `비전 탄막(Barrage) \| 스펜더 \| 즉시` | 고중첩 소모로 창폭              |
|   3 | AETH\_READY\_AND\_NP\_EQ\_1  | **Aether Missiles 대기** & **NP=1**    | `비전 탄막(Barrage) \| 스펜더 \| 즉시` | Aether→마지막 NP 소모 연계     |
|   4 | ORB\_OK\_AND\_CH\_LT\_3      | **Orb 사용 가능** & **충전 < 3**           | `비전 구슬(Arcane Orb) \| 빌더 \| 즉시` | 충전 복구 최우선               |
|   5 | NP\_EQ\_0\_AND\_CC\_GE\_1    | **NP=0** & **클리어캐스팅 ≥1**             | `비전 화살(Arcane Missiles) \| 빌더 \| 채널 2.5s(클립)` | NP 재부여 목적               |
|   6 | CH\_IN\_0\_1                 | **충전 0\~1**                          | `비전 폭발(Arcane Explosion) \| 빌더 \| 즉시` | 근접시만 가치↑                |
|   7 | LEYDRINKER\_PROC             | **Leydrinker 발동**                    | `비전 작열(Arcane Blast) \| 빌더 \| 시전 2.25s` | 특수 시너지                  |
|   8 | ELSE\_BLAST                  | 그 외 전부                               | `비전 작열(Arcane Blast) \| 빌더 \| 시전 2.25s` | 기본 채움                   |
|   9 | OOM\_FALLBACK                | **마나 부족(OOM)**                       | `비전 탄막(Barrage) \| 스펜더 \| 즉시` | 응급 회복/순환 재시동            |

> **ToTM/ASoul 마감 규칙**: 해당 창 **마지막 GCD는 반드시 Barrage**로 마감(거짓 금지 규칙).

### 4.2 루프 플로우(복붙용)

```mermaid
flowchart TD
    subgraph "ST 우선순위 루프"
        direction TB
        A((START LOOP)) --> P1{"<font size=4><b>🚨 긴급 처리</b></font><br>Intuition/Tempo 만료 임박?"};
        P1 -- "예" --> BARR_EMG(["<b>비전 탄막 (Barrage)</b><br>Tempo 유지를 위해 0충전도 허용"]);
        P1 -- "아니오" --> P2;

        subgraph "<b>[1순위] 비전 탄막 (Barrage) 사용 결정</b>"
            direction TB
            P2{"Harmony ≥ 12 & NP ≤ 1?"} -- "예" --> BARR_HARM(["<b>비전 탄막 (Barrage)</b><br>고중첩 하모니 소모"]);
            P2 -- "아니오" --> P3{"Aether Missiles 대기 & NP = 1?"};
            P3 -- "예" --> BARR_AETHER(["<b>비전 탄막 (Barrage)</b><br>Aether/NP 연계"]);
        end

        P3 -- "아니오" --> P4;
        
        subgraph "<b>[2순위] 자원(충전/NP) 생성 결정</b>"
            direction TB
            P4{"충전 < 3 & Orb 사용 가능?"} -- "예" --> ORB(["<b>비전 구슬 (Orb)</b><br>충전 즉시 회복"]);
            P4 -- "아니오" --> P5{"NP = 0 & 클리어캐스팅 보유?"};
            P5 -- "예" --> MISS(["<b>비전 화살 (Missiles)</b><br>NP 확보 (클리핑)"]);
        end

        P5 -- "아니오" --> P6;

        subgraph "<b>[3순위] 필러(Filler) 및 기타</b>"
            direction TB
            P6{"충전 0~1 & 근접?"} -- "예" --> EXPL(["<b>비전 폭발 (Explosion)</b><br>임시 충전 수급"]);
            P6 -- "아니오" --> BLAST(["<b>비전 작열 (Blast)</b><br>기본 필러"]);
        end
        
        subgraph "<b>[최후] 비상 상황</b>"
            direction TB
            BLAST --> OOM{"마나 부족(OOM)?"};
            OOM -- "예" --> BARR_OOM(["<b>비전 탄막 (Barrage)</b><br>응급 순환 재시동"]);
        end
    end

    BARR_EMG --> Z((END CYCLE));
    BARR_HARM --> Z;
    BARR_AETHER --> Z;
    ORB --> Z;
    MISS --> Z;
    EXPL --> Z;
    BARR_OOM --> Z;
    OOM -- "아니오" --> Z;
    Z --> A;

    %% 스타일 정의
    style A fill:#eee,stroke:#333,stroke-width:2px
    style Z fill:#eee,stroke:#333,stroke-width:2px
    
    classDef spender fill:#ffb3ba,stroke:#c1121f,stroke-width:2px,color:#000
    classDef builder fill:#baffc9,stroke:#008000,stroke-width:2px,color:#000
    classDef filler fill:#bae1ff,stroke:#0077b6,stroke-width:2px,color:#000
    classDef emergency fill:#ffffba,stroke:#e9c46a,stroke-width:2px,color:#000

    class BARR_EMG,BARR_HARM,BARR_AETHER,BARR_OOM spender
    class ORB,MISS,EXPL builder
    class BLAST filler
```

---

## 5) 실행 체크리스트(전투 리허설용)

- [ ] **T–4s Evocation 채널**, **T–1s Missiles(클립)**으로 NP 확보.
- [ ] **Surge → (4충전 보장) → Barrage → (비행 중) Touch(Off-GCD) → Barrage** 순서 무오류.
- [ ] **대쿨 직후 Shifting Power 채널**로 CDR 확보.
- [ ] 루프에서 **Intuition/Tempo 만료 손실 0회**.
- [ ] **Harmony ≥\~12** 상황에서 **Barrage 지연 없음**.
- [ ] **Aether Missiles만 풀채널**, 그 외 **클립** 일관성 유지.
- [ ] **ToTM/ASoul 마감 Barrage** 누락 없음.
- [ ] 마나 급감 시 **응급 Barrage**로 순환 재시동.

---

## 6) 참고 운용 메모

- **거리·투사체**: Barrage **비행 시간**을 이용해 **Touch 선-적용**을 안정화.
- **Tempo 예외**: 가속 중첩이 **끊기기 직전**이라면 **0충전 Barrage로 갱신** 허용(표기된 예외만).
- **Orb-in-air**: 이미 **오브가 날아오는 중**이면 **직접 Orb 사용 지연** 후 Barrage로 **충전 손실 없이** 연쇄.

---

## 7) 변경 이력

- **v1.0.0**: 초판 — ST 오프너/루프 표·플로우 확정, Tempo 예외/마감 규칙 명문화.

```text
::contentReference[oaicite:0]{index=0}
```
