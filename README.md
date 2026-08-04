<div align="center">

<br/>

# PipeFlow

### 3D 배관 설계부터 발주 · 결제 · 생산 관리까지, 하나의 흐름으로

종이 도면과 전화·팩스로 오가던 배관 자재 발주 과정을<br/>
**설계 → 견적 → 발주 → 결제 → 상태 관리** 단일 웹 워크플로우로 통합한 B2B 협업 플랫폼

<br/>

![Nuxt](https://img.shields.io/badge/Nuxt_4-00DC82?style=flat-square&logo=nuxt&logoColor=white)
![Vue](https://img.shields.io/badge/Vue_3-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Three.js](https://img.shields.io/badge/Three.js-000000?style=flat-square&logo=threedotjs&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL_16-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy_2.0-D71F00?style=flat-square&logo=sqlalchemy&logoColor=white)

<br/>

</div>

## 왜 만들었나

배관 자재 유통 현장의 발주 프로세스는 여전히 아날로그입니다.

| 단계 | 기존 방식 | 발생하는 비용 |
|---|---|---|
| 설계 | 현장 손 스케치 | 사양이 비구조화 데이터로만 남음 |
| 견적 | 자재 수량 수기 산출 + 단가표 대조 | 사양 하나만 바뀌어도 전 과정 반복 |
| 발주 | 도면 사진 · 전화 · 팩스 | 규격 오전달 → 절단 오차 → 재제작 |
| 확인 | "제작 들어갔나요?" 전화 | 양측 모두 반복 응대 비용 |
| 정산 | 견적/발주 시점 단가 불일치 | 금액 분쟁 |

**PipeFlow의 접근** — 도면을 *그림*이 아니라 **데이터**로 다룹니다.
브라우저에서 설계한 3D 배관은 구조화된 형상 데이터로 저장되고, 그 데이터가 그대로 BOM · 견적 · 발주서 · 2D 도면으로 파생됩니다.

<br/>

## 저장소

| 저장소 | 담당 | 스택 |
|---|---|---|
| **[Frontend](https://github.com/Pipe-Order/Frontend)** | 3D 설계 워크스페이스 · 2D 도면 생성 · 발주 UI · 어드민 콘솔 · **BFF 프록시** | Nuxt 4 · Vue 3 · TypeScript · Three.js · Pinia · Tailwind v4 |
| **[Backend](https://github.com/Pipe-Order/Backend)** | 견적 엔진 · 발주/결제 · 조직 · 관리자 API | FastAPI · SQLAlchemy 2.0 · PostgreSQL 16 · Alembic |

<br/>

## 전체 아키텍처

```
┌──────────────────────────────────────────────────────────────┐
│  Browser                                                     │
│  · 항상 현재 origin 기준 상대경로만 호출                        │
│  · 백엔드 주소를 알 필요가 없음                                 │
└───────────────────────────┬──────────────────────────────────┘
                            │  /nuxt-api/**
┌───────────────────────────▼──────────────────────────────────┐
│  Nuxt 4 Server  (BFF)                        [Frontend 저장소]│
│  · 토큰을 httpOnly 쿠키로 보관 → XSS로 탈취 불가                │
│  · Toss 시크릿 등 서버 전용 값 격리                             │
│  · 랜딩만 SSR, 인증 구역은 CSR                                 │
└───────────────────────────┬──────────────────────────────────┘
                            │  내부망 (localhost:8000)
┌───────────────────────────▼──────────────────────────────────┐
│  FastAPI                                        [Backend 저장소]│
│    Router ──▶ Service ──▶ Model                              │
│                  └──▶ pricing.py (ORM 미의존 순수 함수)         │
└───────────────────────────┬──────────────────────────────────┘
                            │
┌───────────────────────────▼──────────────────────────────────┐
│  PostgreSQL 16    19 tables · 56 migrations                  │
└──────────────────────────────────────────────────────────────┘

외부 연동 : Toss Payments(가상계좌) · 국세청 사업자 상태조회 · AWS S3 · Sentry · Cloudflare Turnstile
```

### 설계 원칙

1. **브라우저는 백엔드 주소를 몰라야 한다** — 배포 IP가 바뀌거나 도메인이 없어도 프론트 수정 불필요
2. **금액과 권한은 서버만 결정한다** — 클라이언트가 보낸 금액은 위·변조 감지용 대조에만 사용
3. **확정된 데이터는 불변이다** — 설계(가변) → 스냅샷(불변) → 발주(박제) 3단 구조

<br/>

## 데이터 흐름

```
 ┌─────────┐  설계   ┌──────────┐  확정   ┌───────────┐
 │ Project │────────▶│ Snapshot │────────▶│   Order   │
 │ (가변)   │         │ (불변)    │         │  (DRAFT)  │
 └─────────┘         └──────────┘         └─────┬─────┘
  pipe_list           도면 박제                   │  견적 계산 + 24h 가격 동결
  JSONB               product_no                │
  편집 락                                        ▼
                                          ┌───────────┐
                                          │  PENDING  │  finalize
                                          └─────┬─────┘  · 사업자 인증 검증
                                                │        · 요금 스냅샷 박제
                                                ▼
                                          ┌───────────┐
                                          │  Payment  │  Toss 가상계좌
                                          └─────┬─────┘
                                    webhook ↓ (본문 미신뢰 · API 재조회)
                                          ┌───────────┐
                                          │ CONFIRMED │──▶ PROCESSING ──▶ SHIPPING ──▶ COMPLETED
                                          └───────────┘              (어드민이 상태 관리)
```

<br/>

## 핵심 기능

<table>
<tr>
<td width="50%" valign="top">

**🧊 브라우저 3D 배관 설계**

직관 · 엘보(45°/90°) · 티 · 크로스 · 리듀서 · 캡을 조립하는 실측 mm 단위 모델링. 혼합 구경(mixed OD) 지원. 지오메트리 계산은 장면 상태를 모르는 **순수 함수**라 렌더러 없이 단위 테스트합니다.

</td>
<td width="50%" valign="top">

**📐 3각법 2D 도면 자동 생성**

3D 모델에서 정면도 · 평면도 · 우측면도 · 등각투상도를 자동 렌더링. **3D 파츠 지오메트리를 그대로 재사용**하고 재질만 교체해 형상 계산 코드를 두 번 작성하지 않습니다.

</td>
</tr>
<tr>
<td width="50%" valign="top">

**🧮 BOM · 견적 자동 산출**

도면 데이터를 규격별로 집계해 자재비 + 용접비(조인트 수 기반 누진 요율)를 계산. 계산 엔진은 **ORM 미의존 순수 함수**라 DB 없이 금액 회귀 테스트가 가능합니다. **24시간 가격 동결**.

</td>
<td width="50%" valign="top">

**💳 발주 · 결제**

국세청 **사업자 진위확인**을 통과한 회사만 발주 확정 가능. Toss Payments 가상계좌 연동. 확정 시점의 요금을 박제해 **이후 정책이 바뀌어도 주문 금액 불변**.

</td>
</tr>
<tr>
<td width="50%" valign="top">

**🛠 어드민 대시보드**

주문 상태 · 회사 인증 · 단가 · 재고 · 감사 로그 관리. 배송비 등 운영 설정을 **코드 재배포 없이** 변경. **TOTP 2단계 인증**을 통과해야 발급되는 20분 단명 세션으로만 접근 가능.

</td>
<td width="50%" valign="top">

**👥 협업**

회사 단위 워크스페이스(OWNER / MANAGER / MEMBER). **TTL 기반 편집 락**으로 동시 편집 충돌 방지, 90초 하트비트로 유령 락 자동 해제. 비로그인 read-only 공유 링크.

</td>
</tr>
</table>

<br/>

## 기술적으로 공들인 지점

<details>
<summary><b>비즈니스 불변식은 애플리케이션이 아니라 DB가 강제한다</b></summary>

<br/>

"취소되지 않은 주문은 도면당 1건"은 서비스 코드가 아니라 **부분 유니크 인덱스**의 일입니다.

```sql
CREATE UNIQUE INDEX ux_orders_active_snapshot_id ON orders (snapshot_id)
  WHERE snapshot_id IS NOT NULL AND status != 'CANCELLED';

CREATE UNIQUE INDEX ux_payments_active_order_id ON payments (order_id)
  WHERE status NOT IN ('CANCELED', 'EXPIRED', 'FAILED');
```

애플리케이션 선검사는 **친절한 에러 메시지(UX)** 를 위한 것이고, 진짜 방어선은 DB 제약입니다. 선검사를 동시에 통과한 두 요청 중 하나는 반드시 `IntegrityError`가 나며, 이를 500이 아닌 **409 Conflict**로 변환합니다.

</details>

<details>
<summary><b>하이브리드 저장 전략 — JSONB와 정규화를 언제 나누는가</b></summary>

<br/>

**형상 데이터는 JSONB** — 부품 타입마다 필요한 필드가 다릅니다(엘보엔 각도, 티엔 분기경). 정규화하면 NULL 컬럼이 폭증하는 EAV 안티패턴에 빠지고, 새 부품 타입 추가마다 마이그레이션이 필요합니다. 조회 단위가 항상 "프로젝트 전체"라 조인 불가라는 단점이 문제되지 않습니다.

**규격 · 단가 카탈로그는 정규화** — 조인·집계가 필요하고 무결성이 금액과 직결됩니다. 배타적 FK, 원산지 차원, 규격 조합 유니크를 모두 DB 제약으로 강제합니다.

</details>

<details>
<summary><b>토큰 로테이션과 가용성의 트레이드오프</b></summary>

<br/>

다중 탭에서 동시에 refresh를 호출하면 근거 없이 로그아웃되는 버그가 있었습니다. 먼저 커밋한 쪽이 토큰 값을 바꿔버려, 뒤늦게 잠금을 얻은 요청의 WHERE 조건이 성립하지 않았기 때문입니다.

> **행 잠금은 *순서*를 보장할 뿐 *조건의 유효성*을 보장하지 않는다.**

직전 토큰 해시와 로테이션 시각을 남기고 **8초 유예 창** 내 도착한 구 토큰은 재로테이션 경로를 타게 해결했습니다. 유예 창 밖의 재사용은 여전히 401이므로 탈취 방어는 그대로 유지됩니다.

</details>

<details>
<summary><b>외부 API 장애가 서비스 전체를 멈추지 않게</b></summary>

<br/>

연속 5회 실패 시 30초간 회로를 여는 서킷 브레이커를 Toss·국세청 호출에 적용했습니다. 외부 장애 시 요청 스레드가 타임아웃까지 붙잡히면 **스레드풀이 고갈되어 서비스 전체가 멈추기** 때문입니다.

재시도는 **멱등성 기준**으로 나눴습니다 — 조회(GET)는 1회 재시도, **결제 승인(POST)은 재시도하지 않습니다**(타임아웃 후 서버측 성공 가능성 → 이중 승인 위험. 최종 상태는 웹훅이 수렴).

또한 네트워크 타임아웃은 결제 실패가 아니므로 `FAILED`로 마킹하지 않고 503으로 응답합니다. **"실패"와 "판정 불가"를 구분**하지 않으면 정상 결제를 유실합니다.

</details>

<details>
<summary><b>인가 누락이 구조적으로 불가능한 설계</b></summary>

<br/>

경로 파라미터로 리소스 ID를 받는 엔드포인트는 반드시 "리소스 로딩 + 인가"를 한 몸으로 묶은 FastAPI 의존성을 사용합니다.

```python
@router.post("/{project_id}/save-canvas")
def save_canvas(project: Project = Depends(get_editable_project), ...):
```

인가 호출을 "잊는" 실수가 **함수 시그니처 차원에서 차단**되고, 역할 × 리소스 조합은 인가 매트릭스 테스트로 전수 검증합니다.

</details>

<br/>

## 프로젝트 규모

<div align="center">

| 백엔드 | 프론트엔드 | DB | 테스트 |
|:---:|:---:|:---:|:---:|
| **~16,000** LOC | **~50,000** LOC | **19** tables | **47** files |
| Python | TypeScript / Vue | 56 migrations | pytest 28 · Vitest 19 |

</div>

**품질 자동화** — push/PR마다 GitHub Actions가 lint · unit test · 프로덕션 빌드를 검증하고, pre-commit(Black · isort · Ruff)이 커밋 단계에서 스타일·정적 오류를 차단합니다. 1인 개발의 리뷰어 부재를 자동화로 보완했습니다.

<br/>

## 확장 시 재방문 지점

rate limiter · 참조 캐시 · 서킷 브레이커는 **단일 프로세스 전제**로 구현되어 있습니다. 지금 필요하지 않은 것을 미리 만들지 않되, 한계와 그 판단 기준은 문서로 남겼습니다.

- [ ] Redis 이전 (rate limit · 캐시 · 서킷 브레이커 상태 공유) → 다중 인스턴스 스케일아웃
- [ ] 구조화(JSON) 로깅 + Prometheus 메트릭
- [ ] 초대형 도면 견적 계산의 비동기 처리 (실측 병목 확인 후)
- [ ] RDS · S3 전환

<br/>

<div align="center">
<sub>기획 · 설계 · 개발 · 운영 단독 수행</sub>
</div>
