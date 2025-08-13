# 08 — Skill-Blocks-and-Timing (스킬 사전 & 시전/채널 타이밍)

> 대상: **WoW 11.2 · 비전 법사(Arcane) · Sunfury · Mythic+**  
> 목적: 앞선 문서(03~07)의 표·플로우에서 참조하는 **표준 스킬 블록**을 **거짓 없이 단일 규격**으로 정의.  
> 원칙: **한글 스킬명 (영문명) · 성격 · 시전/채널/즉시 시간**을 반드시 표기. **투사체/채널/오프-GCD/충전 상호작용**을 명시.

---

## 1) 표준 스킬 블록 정의 표

> 표기 형식: `한글 스킬명 (영문명) | 성격 | 시전시간`  
> 부가 열: `GCD/OffGCD · 쿨다운 · 충전 사용/획득 · 핵심 상호작용/메모`

### 1.1 핵심 공격/자원 스킬

| ID | 스킬 블록 | GCD | 쿨다운 | 충전(Arcane Charges) | 핵심 상호작용 / 메모 |
|---:|---|---|---|---|---|
| S_BLAS | **비전 작열 (Arcane Blast) \| 빌더 \| 시전 2.25s** | GCD | — | **사용 X / 충전+1** | **충전 수에 따라 피해↑**. **NP(네더 프리시전) 소비 대상**. ST 루프의 기본 채움. |
| S_MISS | **비전 화살 (Arcane Missiles) \| 빌더 \| 채널 2.5s** | GCD | — | **사용 X / (특성·상태에 따라 충전 보정)** | **Aether Missiles만 풀채널**, 그 외 **클립**해 **NP 부여**. **Gucci Missiles**(채널 틱 공중 유지 → Barrage 직후 충전 복원) 가능. |
| S_EXPL | **비전 폭발 (Arcane Explosion) \| 빌더 \| 즉시** | GCD | — | **사용 X / 충전+1(범위)** | **근접**에서 **0~1(ST) / 0~3(AoE)** 충전 구간 채움. |
| S_ORB | **비전 구슬 (Arcane Orb) \| 빌더 \| 즉시** | GCD | 짧음 | **사용 X / 통과 대상마다 충전+1** | **<3 충전 시 최우선 빌더**. **Orb Barrage 프록/비행** 관측 시 **직접 사용 보류** 후 **Barrage 연속**으로 이득. |
| S_BARR | **비전 탄막 (Arcane Barrage) \| 스팬더 \| 즉시** | GCD | — | **기본: 4충전 소모** | **기본 원칙: 4충전 사용**. **Tempo 만료 방지 예외**로 **0충전도 허용**. **Intuition/Harmony/GI/오브 프록** 반응. **ToTM/ASoul ‘마감’** GCD. 투사체 **비행시간** 존재. |

### 1.2 창·대쿨·보조

| ID | 스킬 블록 | GCD | 쿨다운 | 자원/버프 | 핵심 상호작용 / 메모 |
|---:|---|---|---|---|---|
| S_TOTM | **마법의 손길 (Touch of the Magi) \| 쿨다운/창구 \| 즉시(Off-GCD)** | **Off-GCD** | 짧음 | 누적·폭발 창 | **반드시** `Barrage 발사 → (비행 중) Touch(Off-GCD)` **선-적중**. **창 마지막 GCD= Barrage**. **(AoE) 창 안 Blast 1회(Spark)** 필요. |
| S_SURG | **비전 폭발력 (Arcane Surge) \| 쿨다운/버스트 \| 시전 1.5s** | GCD | 김 | 주문력/마나 회복 | **대쿨**. 보통 **Touch 직전**. **종료 후 Arcane Soul 발동**(Sunfury). |
| S_EVO | **환기 (Evocation) \| 쿨다운/버프 시동 \| 채널 3.0s** | GCD | 김 | 시전 중 버프/마나 | **T–4s 프리채널** 권장. **(특성) 이동 채널 가능 여부 주의**. |
| S_SPWR | **시프팅 파워 (Shifting Power) \| 보조/CDR \| 채널 4.0s(유지)** | GCD | 중 | 재사용 대기시간 감소 | **대쿨 직후** 채널해 CD 회전. Shimmer로 **채널 유지-이동** 가능. |

### 1.3 패시브·프록(참조 키)

| 키 | 이름 | 효과 요지 | 운용 규칙(핵심) |
|---:|---|---|---|
| NP | **네더 프리시전 (Nether Precision)** | Missiles 사용 시 **다음 Blast/Barrage 강화(2회)** | **NP를 ‘겹치지 않게’ 소모**. Aether만 풀채널, 그 외 **클립**으로 NP 획득. |
| AETH | **에테르 조율 (Aether Attunement)** | **3번째 Missiles 강화+광역** | **해당 Missiles만** 풀채널. 직후 **Barrage 우선** 분기 존재. |
| INTU | **직감 (Intuition)** | **강화 Barrage + 충전 환급** | **만료 임박(다음 GCD 전)**이면 **즉시 Barrage**. |
| TEMPO | **비전 가속 (Arcane Tempo)** | **Barrage로 가속 중첩 획득/유지** | **중첩 만료 임박** 시 **0충전 Barrage 허용**. |
| HARM | **비전 조화 (Arcane Harmony)** | Missiles 적중 누적 → **Barrage 증폭(최대치)** | **≈12 이상**이면 **Barrage 우선**. |
| GI | **영광의 광휘 (Glorious Incandescence)** | Sunfury 핵심 — **Barrage 후 충전 복원** | **ToTM ≤6s**면 **보유한 창 대기 → Touch에 결합** 가치↑. |
| ASOUL | **비전화(Arcane Soul)** | Surge 종료 후 **충전 소모 없이 Barrage 연쇄** | 창 동안 **Barrage 기본값**, **마감 Barrage**, 종료 직후 **Shifting Power**. |

> 주: 위 키들은 본 문서 03~07의 테이블/플로우 **조건문 이름**과 동일.

---

## 2) 시간·투사체·채널 특성 요약

| 요소 | 관련 스킬 | 디테일 | 실전 메모 |
|---|---|---|---|
| **투사체 비행** | Barrage | 적중까지 **가시적 비행시간** | **Touch 선-적중**: Barrage 발사 → **0.2~0.8s 내 Touch(Off-GCD)** |
| **채널 틱** | Missiles, Shifting Power | **틱 단위**로 판정 | **클립은 틱 마감에 맞춰**. Missiles 공중틱을 **Barrage 직후 충전 복원**에 활용(Gucci). |
| **마감 GCD** | ToTM, Arcane Soul | 창 종료 전 **마지막 GCD 예약** | **Barrage로 마감**(거리 멀면 **Blink로 즉착** 확정). |
| **프리채널** | Evocation | 풀 T–4s 권장 | 버스트 전 **안정적/완주 채널** → 이동 포지션 확보. |

---

## 3) 기능별 분류표(결정 트리용 라벨)

| 라벨 | 포함 스킬 | 라벨 정의 | 사용 예 |
|---:|---|---|---|
| BUILDER_FAST | Orb / Explosion | 충전 복구/확보를 **최우선**으로 하는 빌더 | **CH<3 → Orb**, (AoE 근접) **CH≤3 → Explosion** |
| BUILDER_NP | Missiles | **NP 부여/유지**를 위한 빌더 | **NP=0 & CC≥1 → Missiles(클립)** |
| BUILDER_FILL | Blast | 루프의 **기본 채움** | **우선 조건 없음 → Blast** |
| SPENDER_MAIN | Barrage | 모든 창/루프의 **핵심 스팬더** | **HARM↑/INTU/TEMPO 임박/ASOUL/마감**에서 우선 |
| WINDOW_ENTRY | Surge / Touch / Evocation | 창/대쿨의 **진입** | **Barrage 선발사 → Touch(Off-GCD)** |
| CDR | Shifting Power | 대쿨 회전 | **버스트 직후 채널** |

---

## 4) 스킬-상호작용 사전(실전 핵심 10선)

1) **Barrage 선-발사 → (비행 중) Touch(Off-GCD)** = **ToTM 선-적용**(필수).  
2) **창의 마지막 GCD는 Barrage**(ToTM/ASoul 공통).  
3) **Aether Missiles만 풀채널**, 그 외 Missiles는 **NP 부여용 클립**.  
4) **HARM ≈12+**에서 **Barrage 우선**(지연 금지).  
5) **INTU/TEMPO 만료 임박** = **즉시 Barrage**(**Tempo 예외**로 **0충전**도 허용).  
6) **CH<3 & Orb 가능** = **Orb 최우선**. 이미 **오브 비행/프록**이면 **직접 Orb 보류** + **연속 Barrage**.  
7) **(AoE) ToTM 창 내 Blast 1회**로 **Magi’s Spark** 충족.  
8) **Gucci Missiles**: Missiles 틱이 **공중**일 때 **Barrage** → 남은 틱으로 충전 즉시 복원.  
9) **Shifting Power**는 **버스트 직후** 채널해 **CD 회전**(Shimmer로 채널 유지 이동).  
10) **Evocation**은 **T–4s 프리채널**로 시작, 이후 **Surge→Barrage(비행 중)Touch** 순.

---

## 5) 결정 트리(요약 Mermaid) — “무엇을 누를까?”

> 입력: 현재 충전(CH), 버프/프록(HARM/INTU/TEMPO/NP/AETH), 창 상태(ToTM/ASOUL), 오브(프록/비행), 마나.  
> 출력: **라벨 → 실제 스킬**.

```mermaid
flowchart TD
  A(("Decision Root")) --> B["창 상태?"]
  B -- "ToTM 진입 직전" --> T1["SPENDER_MAIN = Barrage\n(선-발사)"] --> T2["WINDOW_ENTRY = Touch(Off-GCD)\n<명중 전>"]
  B -- "ASoul 진행 중" --> S1["SPENDER_MAIN = Barrage\n(연쇄 기본)"]
  B -- "일반 루프" --> C{"INTU/TEMPO 만료 임박?"}
  C -- "예" --> BARR1["SPENDER_MAIN = Barrage\n(0충전 허용)"]
  C -- "아니오" --> D{"HARM ≥~12?"}
  D -- "예" --> BARR2["SPENDER_MAIN = Barrage"]
  D -- "아니오" --> E{"CH < 3 & Orb 가능?"}
  E -- "예" --> ORB["BUILDER_FAST = Orb"]
  E -- "아니오" --> F{"NP = 0 & CC ≥ 1?"}
  F -- "예" --> MISS["BUILDER_NP = Missiles(클립; Aether=풀)"]
  F -- "아니오" --> G{"CH ≤ 1(ST)/≤3(AoE) & 근접?"}
  G -- "예" --> EXPL["BUILDER_FAST = Explosion"]
  G -- "아니오" --> H["BUILDER_FILL = Blast"]
  %% 창 마감
  T2 --> T3["SPENDER_MAIN = Barrage\n(창 마감 GCD)"]
  S1 --> S2{"ASoul 종료 임박?"}
  S2 -- "예" --> S3["SPENDER_MAIN = Barrage\n(창 마감 GCD)"]
````

---

## 6) 암기용 미니 카드(현장용)

- **ST 오프너**: `Evocation(T–4s) → Surge → (CH<4면 Orb) → Barrage → (비행 중) Touch → Barrage → Shifting Power`
- **AoE 오프너**: `Evocation → Surge → (CH<3면 Orb) → Barrage → (비행 중) Touch → Barrage → Missiles(Aether면 풀) → Blast(창 내 1회) → Barrage → Shifting Power`
- **루프 1순위**: `INTU/TEMPO 만료 직전 Barrage` · `HARM≈12+ Barrage` · `CH<3 Orb` · `NP=0&CC≥1 Missiles(클립)`
- **항상**: **창 마감은 Barrage**, **Aether만 풀채널**, **오브 비행/프록 보이면 연속 Barrage**.

---

## 7) 변경 이력

- **v1.0.0**: 초판 — 스킬 블록/시간/상호작용 표준화, 결정 트리 라벨 정의, 요약 플로우 포함.
