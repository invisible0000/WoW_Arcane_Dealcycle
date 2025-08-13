# 09 — CD-Syncing-and-Use-on-CD (쿨다운 동기/보류 의사결정)
> 대상: **WoW 11.2 · 비전 법사(Arcane) · Sunfury · Mythic+**  
> 목적: **Evocation → Surge → Touch → Shifting Power**의 **동기/보류(send vs hold)** 결정을 **거짓 없이 재현**하도록 규칙·표·플로우로 표준화한다.  
> 원칙 요약: **사용 횟수 손실 금지(Use-Count First)**, **Touch 선-적중**, **대쿨 직후 Shifting Power**, **GI(Glorious Incandescence)·Add 타이밍 결합 가치 인지**.

---

## 1) 배경 · 구성 목적
- **왜**: 같은 실력에서도 **대쿨 동기 판단**에 따라 **전투당 Surge 사용 횟수**가 바뀌고, 이는 **총 피해량의 상한**을 결정한다.  
- **무엇을**: **“사용 횟수 우선” 원칙**을 축으로, **보류가 이득인 상황**만 명시해 **안전하게 자동화 가능한 판단체계**를 제공한다.

---

## 2) 본 문서에서 사용하는 스킬(블록 표기)
> 형식: `한글 스킬명 (영문명) | 성격 | 시전시간`

| ID | 스킬 블록 | 핵심 상호작용/메모 |
|---:|---|---|
| S_EVO | **환기 (Evocation) | 쿨다운/버프 시동 | 채널 3.0s** | **T–4s 프리채널**로 대버스트 준비 |
| S_SURG | **비전 폭발력 (Arcane Surge) | 쿨다운/버스트 | 시전 1.5s** | 종료 후 **Arcane Soul** 활성 |
| S_TOTM | **마법의 손길 (Touch of the Magi) | 쿨다운/창구 | 즉시(Off-GCD)** | **Barrage 선-발사 → (비행 중) Touch** 필수 |
| S_SPWR | **시프팅 파워 (Shifting Power) | 보조/CDR | 채널 4.0s(유지)** | **버스트 직후** 채널로 CD 회전 |
| S_BARR | **비전 탄막 (Arcane Barrage) | 스펜더 | 즉시** | 모든 창 **마감 GCD**. Tempo 만료 보호용 예외(0충전 허용) |

---

## 3) “사용 횟수 우선(Use-Count First)” 규칙
- **정의**: 보스/네임드·대형 풀 기준 전투 동안 **Surge 예상 사용 횟수**가 **보류로 1회라도 줄어들면** → **절대 보류 금지**, **즉시 사용(send on CD)**.  
- **이득 보류**의 조건은 아래 **결합 가치** 중 하나라도 **획득**하면서도 **사용 횟수 감소가 0**일 때만 허용.

---

## 4) 결합 가치(보류 고려 사유) — 우선순위 매트릭스

| 우선 | 트리거 | 사람이 읽는 조건 | 행동 |
|---:|---|---|---|
| 1 | **ADD_SYNC** | **30s 이내 새 웨이브/쉴드/취약 페이즈** 확정 | **해당 타이밍까지 보류**, 이후 **Evo→Surge→(4충전)Barrage→(비행 중)Touch** |
| 2 | **GI_TOUCH_6S** | **Glorious Incandescence 보유** & **Touch 재사용 ≤6s** | **Touch까지 보류**, **Touch 선-적중 스크립트**로 결합 |
| 3 | **EVO_6S** | **Evocation 재사용 ≤6s** | **Evo까지 보류**, **T–4s 프리채널 후 Surge** |
| 4 | **BLOODLUST_SOON** | **영웅심(블러드) 15s 이내 예정** | **블러드까지 보류**(사용 횟수 손실 0일 때) |
| 5 | **TANK_PULL_GAP** | **다음 풀까지 ≤10s 공백** | **다음 풀 첫 GCD로 대쿨**(위 스크립트 그대로) |
| 6 | **ELSE** | 그 외 전부 | **즉시 사용(send on CD)** |

> **보스별/던전별 스케줄**은 파티가 바뀌면 달라질 수 있음. **손실 없는 보류인지**만 항상 우선 확인.

---

## 5) 창 구성 프리셋(권장 시퀀스)
- **Big Go(대쿨 동기)**: `Evocation(T–4s) → Surge → (CH<4면 Orb) → Barrage → (비행 중) Touch(Off-GCD) → Barrage → Shifting Power`
- **Mini Go(소쿨/Touch 단독)**: `(CH<4면 Orb) → Barrage → (비행 중) Touch → Barrage → (필요 시) Missiles/Blast → 마감 Barrage`

> AoE ToTM 창에는 **Blast 1회(Magi’s Spark)** 반드시 포함(문서 05 참조).

---

## 6) 의사결정 플로우(복붙용)

```mermaid
flowchart TD
  A((CD 의사결정 시작)) --> B{보류 시 Surge\n사용 횟수 감소?}
  B -- 예(감소) --> SEND1[즉시 사용(send on CD)\nEvo→Surge→(4충전)Barrage→(비행 중)Touch→Barrage→SP]
  B -- 아니오(0회) --> C{30s 이내 웨이브/취약(ADD_SYNC)?}
  C -- 예 --> HOLD_ADD[보류→해당 타이밍 진입 시 Big Go 실행]
  C -- 아니오 --> D{GI 보유 & Touch ≤6s(GI_TOUCH_6S)?}
  D -- 예 --> HOLD_T[Touch까지 보류→ToTM 선-적중 스크립트 실행]
  D -- 아니오 --> E{Evocation ≤6s(EVO_6S)?}
  E -- 예 --> HOLD_E[Evo까지 보류→T–4s 프리채널 후 Big Go]
  E -- 아니오 --> F{영웅심 ≤15s(BLOODLUST_SOON)?}
  F -- 예 --> HOLD_BL[블러드까지 보류(사용 횟수 손실 0일 때)]
  F -- 아니오 --> G{다음 풀 ≤10s(TANK_PULL_GAP)?}
  G -- 예 --> HOLD_PULL[다음 풀 첫 GCD로 Big Go 실행]
  G -- 아니오 --> SEND2[즉시 사용(send on CD)]
````

---

## 7) 타이밍·거리 필수 규칙(재확인)

* **Touch 선-적중**: **Barrage 발사 → 0.2\~0.8s 내 Touch(Off-GCD)** → **Barrage 착탄**.
* **창 마감**: **ToTM/Arcane Soul 종료 직전** **반드시 Barrage**로 마감(필요 시 **Blink**로 근접 즉착).
* **대쿨 직후**: **Shifting Power 채널**로 **재사용 감축**, 다음 주기 당김.

---

## 8) 실패-복구 스크립트

| 실패                 | 즉시 조치                  | 다음 단계                  |
| ------------------ | ---------------------- | ---------------------- |
| 보류로 Surge 사용 1회 손실 | **즉시 send on CD 전환**   | 이후 **보류 금지** 원칙 준수     |
| Touch 선-적중 실패      | **다음 GCD Barrage**로 보정 | 다음 창에서 **거리 단축**하고 재시도 |
| 마나 급감(OOM)         | **Barrage 즉시**         | **Orb**로 충전 복구 → 루프 재개 |

---

## 9) 체크리스트(현장용)

* [ ] **보류 전** 항상 **사용 횟수 손실 여부=0** 확인.
* [ ] **Add/쉴드/취약** 등 **30s 이내 확정 이벤트**만 보류 사유.
* [ ] **GI 보유 & Touch ≤6s**면 **결합 보류**, 그 외엔 **즉시 사용**.
* [ ] **Evo ≤6s**면 **프리채널 후 동기**.
* [ ] **대쿨 직후 Shifting Power** 채널.
* [ ] **Touch 선-적중** 및 **창 마감 Barrage** 유지.

---

## 10) 변경 이력

* **v1.0.0**: 초판 — “사용 횟수 우선” 기반 CD 동기/보류 규칙, 결합 가치 매트릭스, 결정 플로우 확정.
