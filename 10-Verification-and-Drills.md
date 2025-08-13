# 10 — Verification-and-Drills (검증·훈련·로그 리포트)

> 대상: **WoW 11.2 · 비전 법사(Arcane) · Sunfury · Mythic+**  
> 목적: 앞서 작성한 01~09 문서의 **딜 사이클이 실제 전투/더미에서 “거짓 없이” 재현**되는지 **지표(KPI)·연습 드릴·로그 점검**으로 검증한다.  
> 핵심: **Touch 선-적중**, **창 마감 Barrage**, **Intuition/Tempo 만료 보호**, **(AoE) Spark 충족**, **오브 프록 반응**.

---

## 1) 준비(환경·UI·기본값)

- **연습 장소**: **단일 대상 더미 / 다중 대상 더미(3~8개)**  
- **주요 추적(필수)**: `Touch of the Magi`, `Arcane Surge/Arcane Soul`, `Arcane Barrage`, `Arcane Orb`, `Arcane Missiles(Aether/일반)`, `Arcane Tempo`, `Intuition`, `Arcane Harmony`  
- **Spell Queue Window**: **≈300ms** 권장(입력 빠른 편 기준) — GCD 경계 손실 최소화  
- **거리**: **ToTM 창 ‘마감 Barrage’ 즉착**을 위해 **근접 유지**(필요 시 **점멸(Blink)**)

> 본 문서의 스킬 블록 표기: `한글 (영문) | 성격 | 시전/채널/즉시`

---

## 2) KPI(핵심 지표) — “패턴이 맞는가?”

| ID | 지표 | 목표 | 설명/판정 방법(요약) |
|---:|---|---|---|
| K1 | **Touch 선-적중 성공률** | **≥ 95%** | `비전 탄막(Arcane Barrage) \| 스펜더 \| 즉시` **발사 후** `마법의 손길(Touch of the Magi) \| 즉시(Off-GCD)`를 눌러 **Barrage 피해 로그가 Touch 적용 이후**에 찍히면 성공 |
| K2 | **창 마감 Barrage 준수** | **100%** | `ToTM/Arcane Soul` **종료 직전 마지막 GCD**가 항상 `비전 탄막` |
| K3 | **Aether Missiles 풀 채널 준수** | **≥ 98%** | `비전 화살(Arcane Missiles) \| 빌더 \| 채널 2.5s` — **Aether**만 **풀 채널**, 그 외 **클립** |
| K4 | **Intuition 만료 손실** | **0회** | `Intuition` 잔여가 **다음 GCD 전 만료**이면 **즉시 Barrage** |
| K5 | **Arcane Tempo 다운타임** | **≤ 1.0s/분** | `Arcane Tempo` 만료 방지를 위해 **0충전 Barrage 예외** 허용(필요 시) |
| K6 | **(AoE) Spark 충족률** | **100%** | ToTM 창 내 **`비전 작열(Arcane Blast) \| 빌더 \| 시전 2.25s` 1회** 포함(Magi’s Spark) |
| K7 | **오브 과잉/낭비** | **0회** | `비전 구슬(Arcane Orb)` 중첩/프록/비행이 있을 때 **직접 Orb 사용 보류 + Barrage 연속**으로 충전 손실 없음 |
| K8 | **Harmony 고중첩 지연** | **0회** | `Arcane Harmony` **≈12+** 도달 시 **즉시 Barrage** |
| K9 | **OOM 복구 지연** | **≤ 1 GCD** | 마나 급감 시 **즉시 Barrage → Orb**로 재순환 |

---

## 3) 드릴(훈련 시나리오) — 단계별 스크립트

### 3.1 ST 오프너 → 루프(10회 반복)

| STEP | 조건 | 입력 | 판정 포인트 |
|---:|---|---|---|
| 1 | 풀 T–4s | **환기(Evocation) \| 쿨/버프 \| 채널 3.0s** | 프리채널 완주 |
| 2 | 풀 0s | **비전 폭발력(Arcane Surge) \| 쿨/버스트 \| 시전 1.5s** | Surge 타임라인 고정 |
| 3 | CH<4 | **비전 구슬(Arcane Orb) \| 빌더 \| 즉시** | 4충전 확보 |
| 4 | CH=4 | **비전 탄막(Arcane Barrage) \| 스펜더 \| 즉시** | **선-발사** |
| 5 | 탄막 비행 | **마법의 손길(Touch of the Magi) \| Off-GCD** | **K1** 체크 |
| 6 | — | **비전 탄막(Barrage)** | 오프너 2타 |
| 7 | Aether? | **미사일(Missiles)** | Aether=풀, else=클립(**K3**) |
| 8 | — | **비전 작열(Blast)** | NP/채움 |
| 9 | 종료 임박 | **비전 탄막(Barrage)** | **K2** 체크 |
| 10 | — | **시프팅 파워(Shifting Power) \| 채널 4.0s** | 대쿨 회전 |

### 3.2 AoE 오프너(팩 5~8) → 루프(10회 반복)

| STEP | 조건 | 입력 | 판정 포인트 |
|---:|---|---|---|
| 1 | T–4s | **Evocation** | 프리채널 |
| 2 | 0s | **Arcane Surge** | — |
| 3 | CH<3 | **Arcane Orb** | 4충전 |
| 4 | CH=4 | **Arcane Barrage** | 선-발사 |
| 5 | 비행 | **Touch of the Magi(Off-GCD)** | **K1** |
| 6 | — | **Arcane Barrage** | 창 2타 |
| 7 | — | **Arcane Missiles** | Aether=풀, else=클립 |
| 8 | Spark 필요 | **Arcane Blast** | **K6** |
| 9 | — | **Arcane Barrage** | 하모니/프록 반응 |
| 10 | — | **Shifting Power** | 회전 |

---

## 4) 오류 → 교정 매핑(원인·조치)

| 증상/로그 패턴 | 추정 원인 | 즉시 조치 | 재발 방지(버릇) |
|---|---|---|---|
| **Touch 적용이 Barrage 피해보다 늦음** | 선-적중 타이밍 과도 지연 | 다음 오프너에서 **Barrage 직후 0.2~0.8s 내 Touch** | **Touch를 마우스다운 단축키**에 배치(Off-GCD 체감) |
| **창 마감에서 Blast/미사일로 종료** | 마감 GCD 인지 실패 | **즉시 Barrage**로 보정 | **창 종료 2GCD 전부터 Barrage 예약** 습관화 |
| **Intuition 만료 손실** | 버프 시야/우선순위 문제 | **즉시 Barrage** | **만료 3s 경고** 배치, **Tempo와 동일 영역** |
| **Tempo 중첩 빈번 붕괴** | 0충전 예외 미사용 | **0충전 Barrage 허용** | **Tempo 만료 ≤1.0s**면 무조건 Barrage |
| **Aether Missiles 클립** | Aether 식별 실패 | **다음 Aether 풀채널** | **Aether 전용 테두리**/색상 구분 |
| **Orb 과잉(충전 손실)** | Orb 직후 Barrage 타이밍 실패 | **오브 비행 관측 시 Barrage 연속** | **오브 ‘비행 중’ 시각화**(투사체 관찰 습관) |
| **OOM 후 공회전** | Barrage 응급 처리 미흡 | **Barrage → Orb** | **마나 경고** / **Barrage 응급 키** 별도 배치 |

---

## 5) 정량 체크리스트(현장 점검표)

- [ ] **K1 ≥ 95%**(Touch 선-적중)  
- [ ] **K2 = 100%**(창 마감 Barrage)  
- [ ] **K3 ≥ 98%**(Aether만 풀 채널)  
- [ ] **K4 = 0회**(Intuition 만료 손실)  
- [ ] **K5 ≤ 1.0s/분**(Tempo 다운타임)  
- [ ] **K6 = 100%**(AoE Spark 충족)  
- [ ] **K7 = 0회**(Orb 과잉/낭비)  
- [ ] **K8 = 0회**(Harmony 고중첩 지연)  
- [ ] **K9 ≤ 1 GCD**(OOM 복구 지연)

---

## 6) 드릴용 플로우 — “연습 세션 루프”(복붙용)

```mermaid
flowchart TD
  A(("연습 세션 시작")) --> B["ST 오프너 1회 실행"]
  B --> C{"K1(선-적중) ≥95%?"}
  C -- "아니오" --> C1["타이밍 조정(0.2~0.8s)\nBlink로 근접 유지"] --> B
  C -- "예" --> D["ST 루프 5회"]
  D --> E{"K2(마감 Barrage)=100%?"}
  E -- "아니오" --> E1["창 종료 2GCD 전 Barrage 예약 습관"] --> D
  E -- "예" --> F["AoE 오프너 1회"]
  F --> G{"K6(Spark)=100%?"}
  G -- "아니오" --> G1["ToTM 창 내 Blast 1회 강제"] --> F
  G -- "예" --> H["AoE 루프 5회"]
  H --> I{"K4,K5(직감/템포)=OK?"}
  I -- "아니오" --> I1["0충전 Barrage 예외·경고음 보강"] --> H
  I -- "예" --> J{"K7(오브 낭비)=0?"}
  J -- "아니오" --> J1["오브 비행 관측→Barrage 연속"] --> H
  J -- "예" --> K["세션 종료/로그 점검"]
````

---

## 7) 스킬 블록(참조 카드)

| 블록                             | 정의        |                 |
| ------------------------------ | --------- | --------------- |
| \*\*마법의 손길 (Touch of the Magi) | 쿨다운/창구    | 즉시(Off-GCD)\*\* |
| \*\*비전 탄막 (Arcane Barrage)     | 스펜더       | 즉시\*\*          |
| \*\*비전 구슬 (Arcane Orb)         | 빌더        | 즉시\*\*          |
| \*\*비전 화살 (Arcane Missiles)    | 빌더        | 채널 2.5s\*\*     |
| \*\*비전 작열 (Arcane Blast)       | 빌더        | 시전 2.25s\*\*    |
| \*\*시프팅 파워 (Shifting Power)    | 보조/CDR    | 채널 4.0s\*\*     |
| \*\*환기 (Evocation)             | 쿨다운/버프 시동 | 채널 3.0s\*\*     |
| \*\*비전 폭발력 (Arcane Surge)      | 쿨다운/버스트   | 시전 1.5s\*\*     |

---

## 8) 세션 종료 리포트(샘플 템플릿)

```text
세션 일시: YYYY-MM-DD HH:MM
더미/던전: (ST 더미 / AoE 더미X개 / 실던전)
장비/특성: Sunfury · X.X.X
KPI: K1 % / K2 % / K3 % / K4 회 / K5 s/min / K6 % / K7 회 / K8 회 / K9 GCD
주요 실패 패턴: (예: Touch 지연, Tempo 붕괴 등)
개선 액션: (예: Touch 키 위치 변경, Tempo 경고음 추가, Blink 근접 루틴)
다음 목표: (예: K1 97% 달성, K5 0.6s/min 이하)
```

---

## 9) 변경 이력

- **v1.0.0**: 초판 — KPI/드릴/오류-교정/세션 플로우·리포트 템플릿 수록.
