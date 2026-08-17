<div align="center">

<br/>

# PipeFlow

### 3D 배관 설계에서 발주 · 결제 · 생산 관리까지, 끊기지 않는 하나의 흐름

종이 도면과 전화 · 팩스로 오가던 배관 자재 발주를<br/>
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

<!-- ─────────────────────────────────────────────────────────────
     스크린샷 자리. 아래 경로에 이미지를 넣으면 바로 표시된다.
       assets/screenshot-workspace.png   3D 설계 워크스페이스 (권장 1600px 이상)
       assets/screenshot-drawing.png     2D 도면 자동 생성
       assets/screenshot-order.png       발주 · 견적 화면
     ───────────────────────────────────────────────────────── -->

<img src="https://raw.githubusercontent.com/Pipe-Order/.github/main/profile/assets/screenshot-workspace.png" width="900" alt="3D 배관 설계 워크스페이스"/>

<sub>브라우저에서 조립한 3D 배관이 그대로 BOM · 견적 · 2D 도면 · 발주서가 된다</sub>

<br/>
<br/>

<table>
<tr>
<td align="center" width="20%"><b>75,327</b><br/><sub>줄</sub></td>
<td align="center" width="20%"><b>24</b><br/><sub>테이블</sub></td>
<td align="center" width="20%"><b>628</b><br/><sub>테스트 케이스</sub></td>
<td align="center" width="20%"><b>1,042</b><br/><sub>req/s</sub></td>
<td align="center" width="20%"><b>281</b><br/><sub>커밋</sub></td>
</tr>
<tr>
<td align="center"><sub>Python 19,474<br/>TS · Vue 55,853</sub></td>
<td align="center"><sub>마이그레이션 64개<br/>선형 체인, 브랜치 0</sub></td>
<td align="center"><sub>pytest 344<br/>Vitest 284</sub></td>
<td align="center"><sub>p95 37.5 ms<br/>동시성 40, 실패 0</sub></td>
<td align="center"><sub>2026.03 ~ 08<br/>기획부터 운영까지 1인</sub></td>
</tr>
</table>

</div>

<br/>

## 왜 만들었나

배관 자재 제작 현장을 가까이서 지켜볼 기회가 있었다.

눈에 걸린 건 일이 고되다는 점이 아니었다. **회사에서 의사결정 권한이 가장 큰 사람이 하루의 절반을 도로 위에서 쓴다**는 점이었다.

의뢰 한 건이 설치까지 가는 동선은 대략 이렇다.

| | 현장에서 실제로 일어나는 일 | 여기서 새는 것 |
|:---:|---|---|
| 1 | 영업처로 이동해 손 스케치 의뢰서를 받는다 | **왕복 이동.** 사양이 검색도 계산도 안 되는 형태로만 남는다 |
| 2 | 작업장으로 돌아와 도면화하고 자재 수량을 수기 산출한다 | 단가표 대조까지 전부 사람 손 |
| 3 | 다시 영업처로 이동해 도면과 사양을 확인받는다 | **왕복 이동.** 확인 한 줄을 받으러 반나절 |
| 4 | 수정 사항이 나오면 돌아가서 2번부터 다시 한다 | 2–3 구간 전체 반복 |
| 5 | 또 영업처로 이동해 재확인한다 | **왕복 이동** |
| 6 | 절단 · 용접 | 규격 오전달이 여기서 드러나면 자재째 재제작 |
| 7 | 배송 · 설치 | 이후 "제작 들어갔나요" 응대가 양쪽에 계속 |

편도 한두 시간 거리를 한 건에 세 번 이상 오가는 일이 드물지 않다.

그런데 그 이동의 대부분은 **"이 사양이 맞습니까"를 확인하기 위한 것**이다. 정보 한 줄 때문에 반나절이 사라지고, 그 반나절을 쓰는 사람은 대표급이라 같은 시간에 영업도 견적도 하지 못한다. 규격이 하나 어긋나면 3 → 4 → 5가 통째로 다시 돌고, 이미 절단한 자재는 되돌릴 수 없다.

정산 단계에서 견적 시점과 발주 시점의 단가가 어긋나 금액 분쟁이 생기는 것도 같은 뿌리다. 어느 시점의 사양과 단가가 확정본인지 기록이 남지 않기 때문이다.

**PipeFlow가 줄이려는 건 작업이 아니라 이동이다.**

브라우저에서 조립한 3D 배관은 구조화된 형상 데이터로 저장되고, 같은 데이터가 BOM · 견적 · 2D 도면 · 발주서로 파생된다. 영업처는 링크로 도면을 열어 확인하고, 수정 요청은 그 자리에서 반영된다. 확인을 위한 왕복이 사라지면 4번의 재작업 루프도 같이 사라진다.

발주 이후에는 진행 상태가 화면에 있으니 확인 전화도 필요 없다. 확정 시점의 사양과 단가는 스냅샷으로 박제되므로 "그때 얼마로 얘기했었나"를 다투지 않는다.

사람이 옮겨 적는 구간이 사라지면, 옮겨 적다 생기는 오차도 함께 사라진다.

<br/>

## 저장소

| 저장소 | 담당 | 스택 |
|---|---|---|
| **[Frontend](https://github.com/Pipe-Order/Frontend)** | 3D 설계 워크스페이스 · 2D 도면 생성 · 발주 UI · 어드민 콘솔 · **BFF 프록시** | Nuxt 4 · Vue 3 · TypeScript · Three.js · Pinia · Tailwind v4 |
| **[Backend](https://github.com/Pipe-Order/Backend)** | 견적 엔진 · 발주/결제 · 조직/권한 · 관리자 API | FastAPI · SQLAlchemy 2.0 · PostgreSQL 16 · Alembic |

<br/>

## 시스템 아키텍처

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Pipe-Order/.github/main/profile/assets/architecture-dark.png">
  <img src="https://raw.githubusercontent.com/Pipe-Order/.github/main/profile/assets/architecture-light.png" alt="PipeFlow 시스템 아키텍처: Browser → Nuxt 4 BFF → FastAPI → PostgreSQL 16, 외부 연동은 서버 사이드에서만" width="100%">
</picture>

설계를 관통하는 원칙은 세 가지다.

**브라우저는 백엔드 주소를 몰라야 한다.** 클라이언트는 언제나 현재 origin 기준 상대경로만 호출하고, Nuxt 서버가 내부망으로 프록시한다. 배포 IP가 바뀌거나 도메인이 없어도 프론트엔드는 손대지 않는다. 토큰은 httpOnly 쿠키에 있으므로 스크립트가 읽을 수 없고, Toss 시크릿 키 같은 값은 아예 클라이언트 번들에 들어가지 않는다.

**금액과 권한은 서버만 결정한다.** 클라이언트가 보낸 금액은 계산에 쓰이지 않고, 서버 계산값과의 대조(위·변조 감지)에만 쓴다.

**확정된 데이터는 되돌아보지 않는다.** 설계(가변) → 스냅샷(불변) → 발주(박제)의 3단 구조로, 뒤 단계는 앞 단계가 나중에 바뀌거나 삭제돼도 영향을 받지 않는다.

<br/>

## 데이터 흐름

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Pipe-Order/.github/main/profile/assets/dataflow-dark.png">
  <img src="https://raw.githubusercontent.com/Pipe-Order/.github/main/profile/assets/dataflow-light.png" alt="PipeFlow 데이터 흐름: Project → Snapshot → Order 3단 불변화, 8개 상태의 주문 상태 머신, 결제 웹훅 재조회 구조" width="100%">
</picture>

<br/>

## 핵심 기능

<table>
<tr>
<td width="50%" valign="top">

**브라우저 3D 배관 설계**

직관, 엘보(45° / 90°), 티, 크로스, 리듀서, 캡 6종을 조립하는 실측 mm 단위 모델링. 서로 다른 외경을 리듀서로 잇는 혼합 구경도 지원한다. 지오메트리 계산은 Three.js 씬도 Vue 상태도 모르는 순수 함수라, 편집 캔버스와 읽기전용 뷰어가 같은 모듈을 공유하고 렌더러 없이 단위 테스트한다.

</td>
<td width="50%" valign="top">

**3각법 2D 도면 자동 생성**

3D 모델에서 정면도 · 평면도 · 우측면도 · 등각투상도를 자동 렌더링한다. 형상 계산 코드를 두 번 쓰지 않기 위해 **3D 파츠 지오메트리를 그대로 재사용하고 재질만 교체**했다. 모서리 검출 각도는 40°로 맞춰, 곡면 분할선은 감추고 실제 꺾임만 도면선으로 남긴다.

</td>
</tr>
<tr>
<td width="50%" valign="top">

**BOM · 견적 자동 산출**

형상 데이터를 규격별로 집계해 자재비와 용접비를 계산한다. 용접비는 부품별 조인트 수(엘보 2, 티 3, 크로스 4, 리듀서 2, 캡 1)에 구간별 누진 요율을 적용한다. 계산 엔진은 ORM을 import하지 않는 순수 함수라 DB 없이 금액 회귀 테스트 27건이 초 단위로 돈다. 견적가는 **24시간 동결**된다.

</td>
<td width="50%" valign="top">

**발주 · 결제**

국세청 사업자 진위확인을 통과한 회사만 발주를 확정할 수 있다. Toss Payments 가상계좌를 연동했고, 확정 시점의 배송비 · 부가 옵션 요금을 스냅샷으로 박제해 **이후 관리자가 요금 정책을 바꿔도 기존 주문 금액은 변하지 않는다**.

</td>
</tr>
<tr>
<td width="50%" valign="top">

**어드민 콘솔**

주문 상태, 회사 인증, 단가 · 재고, 감사 로그, FAQ, 고객 문의, 광고 배너, 버그 신고를 한 콘솔에서 관리한다. 배송비 같은 운영 설정은 코드 재배포 없이 바꾼다. 대시보드는 미처리 주문 · 미인증 회사 · 미답변 문의 · 미해결 신고 4종을 배지로 띄운다. 접근은 **TOTP 2단계 인증을 통과해야 발급되는 20분짜리 단명 세션**으로만 가능하다.

</td>
<td width="50%" valign="top">

**협업 · 공유**

회사 단위 워크스페이스(OWNER / MANAGER / MEMBER)와 프로젝트 단위 역할(owner / editor / viewer)이 분리돼 있다. 동시 편집 충돌은 **TTL 90초 편집 락**으로 막고, 30초 하트비트로 갱신한다(하트비트를 한두 번 놓쳐도 락이 풀리지 않도록 여유를 뒀다). 비로그인 read-only 공유 링크도 지원한다.

</td>
</tr>
</table>

<br/>

## 설계에서 중요한 결정 8가지

각 항목은 **무엇이 문제였고, 어떻게 막았는가** 순서다.

<details>
<summary><b>1. 동시에 발주해도 주문은 한 건만 생긴다</b></summary>

<br/>

| | |
|---|---|
| **문제** | 같은 도면으로 두 요청이 동시에 들어오면 주문이 2건 생긴다. 코드로 미리 검사해도 두 요청이 그 검사를 동시에 통과할 수 있다 |
| **해결** | 규칙을 코드가 아니라 **DB 인덱스**에 넣었다. 둘 중 하나는 반드시 실패하고, 그 실패를 `409 Conflict`로 바꿔 응답한다 |
| **수치** | 부분 유니크 인덱스 **4개** · CHECK 제약 **10개** |

```sql
CREATE UNIQUE INDEX ux_orders_active_snapshot_id ON orders (snapshot_id)
  WHERE snapshot_id IS NOT NULL AND status != 'CANCELLED';
```

코드 검사는 사용자에게 친절한 메시지를 주기 위한 것이고, 실제 방어선은 DB다.

</details>

<details>
<summary><b>2. 탭을 여러 개 띄워도 로그아웃되지 않는다</b></summary>

<br/>

| | |
|---|---|
| **문제** | 탭 3개가 동시에 토큰을 갱신하면, 늦게 도착한 요청은 이미 교체된 토큰을 들고 있어 거절된다. 사용자에겐 이유 없는 로그아웃으로 보인다 |
| **해결** | 직전 토큰도 짧은 시간 동안은 인정한다. 그 창을 벗어난 재사용은 그대로 401이라 탈취 방어는 유지된다 |
| **수치** | 백엔드 유예 **8초** · BFF grace **60초** · 관련 테스트 **12건** |

동시 요청 병합은 두 겹이다. 같은 탭은 Promise 하나로 합치고, 여러 탭이 동시에 보낸 요청은 Nuxt 서버가 백엔드 호출 **1회**로 합친다.

</details>

<details>
<summary><b>3. 위조된 결제 알림으로 주문 상태를 바꿀 수 없다</b></summary>

<br/>

| | |
|---|---|
| **문제** | 결제 웹훅은 주소만 알면 누구나 보낼 수 있다. 본문을 믿으면 "결제 완료" 한 번에 주문이 확정된다 |
| **해결** | 본문에서 `paymentKey`만 꺼내고, 금액과 상태는 **Toss에 다시 물어본 결과**만 사실로 쓴다. 재조회는 우리 시크릿 키가 있어야 하므로 위조할 수 없다 |
| **수치** | 웹훅 상태 전이 테스트 **5개 시나리오** |

DONE이던 결제가 CANCELED로 돌아오면 환불로 보고, 주문을 자동 취소하지 않고 `PAUSED`로 세워 사람이 판단하게 한다.

</details>

<details>
<summary><b>4. 결제 타임아웃을 "실패"로 처리하지 않는다</b></summary>

<br/>

| | |
|---|---|
| **문제** | 네트워크가 끊기면 결제가 됐는지 안 됐는지 알 수 없다. 이때 실패로 찍으면 **실제로 성공한 결제를 잃는다** |
| **해결** | 타임아웃은 `503`으로 응답하고, 최종 판정은 웹훅에 맡긴다. 승인 요청은 재시도하지 않는다 — 두 번 승인될 위험이 있기 때문 |
| **수치** | 연결 **3초** / 전체 **10초** 제한, 조회 재시도 **1회**, 승인 재시도 **0회** |

외부 API가 계속 실패하면 **5회 연속 실패 시 30초간 호출을 끊는다.** 외부 장애로 요청 스레드가 전부 붙잡히면 서비스 전체가 멈추기 때문이다.

</details>

<details>
<summary><b>5. 권한 검사를 빠뜨릴 수 없는 구조</b></summary>

<br/>

| | |
|---|---|
| **문제** | 엔드포인트가 **116개**다. 한 곳에서 권한 검사를 빠뜨리면 남의 도면이 열린다 |
| **해결** | "리소스를 가져오는 일"과 "권한을 확인하는 일"을 한 덩어리로 묶었다. 권한을 통과하지 못하면 애초에 객체를 손에 넣을 수 없다 |
| **수치** | 역할 × 리소스 전수 테스트 **28케이스** (프로젝트 15 · 회사 멤버 13) |

```python
def save_canvas(project: Project = Depends(get_editable_project)):
```

검사를 잊는 실수가 **함수 서명 단계에서 막힌다.**

</details>

<details>
<summary><b>6. 부품 종류가 늘어도 DB를 고치지 않는다</b></summary>

<br/>

| | |
|---|---|
| **문제** | 엘보엔 각도, 티엔 분기경, 리듀서엔 양쪽 외경이 필요하다. 부품마다 필드가 달라 하나의 표로 만들면 빈 칸이 폭증한다 |
| **해결** | 형상 데이터는 **JSONB**로, 금액에 닿는 규격·단가는 **정규화**로 나눴다. 새 부품이 생겨도 마이그레이션이 없다 |
| **수치** | JSONB **18개 컬럼 / 8개 테이블**, 나머지 전부 정규화 |

기준은 하나다. 조인·집계가 필요하고 **틀리면 돈이 어긋나는** 데이터만 정규화 비용을 낸다. `payments` 삭제 정책만 유일하게 RESTRICT인 것도 같은 이유다.

</details>

<details>
<summary><b>7. 금액 버그는 같은 방식으로 두 번 나지 않는다</b></summary>

<br/>

| | |
|---|---|
| **문제** | 용접비 계산이 구간마다 소수점을 버려서, 청구액이 조금씩 **적게** 나갔다 |
| **해결** | 원 단위 절상으로 정책을 통일하고, 구간 경계값을 회귀 테스트로 고정했다 |
| **수치** | 가격 계산 테스트 **27건**, DB 없이 초 단위로 실행 |

견적 엔진이 DB를 모르는 순수 함수라 픽스처 없이 돈다. 파이프 길이 합산에서 500mm 구간이 누락되던 버그도 같은 방식으로 잡았다.

</details>

<details>
<summary><b>8. 제품 판단이 스키마 이력에 남는다</b></summary>

<br/>

| | |
|---|---|
| **문제** | 배너 광고를 "1번 자리 / 2번 자리" 지정 판매로 만들었는데, 광고주가 한 곳뿐이면 **돈을 받고도 노출이 비는 구간**이 생긴다 |
| **해결** | 노출 로테이션 방식으로 바꾸고 `ad_banners.position` 컬럼을 삭제했다 |
| **기록** | 왜 지웠는지를 마이그레이션 설명에 남겨, 스키마 이력이 곧 판단 이력이 되게 했다 |

</details>

<br/>

## 코드 품질

리뷰어가 없는 1인 개발이라, 사람이 하던 확인을 자동화로 대체했다.

### 커밋 · PR마다 자동으로 도는 것

| | 백엔드 | 프론트엔드 |
|---|:---:|:---:|
| 린트 | ruff | eslint |
| 타입 검사 | — | vue-tsc |
| 테스트 | pytest **344**건 | vitest **284**건 |
| 프로덕션 빌드 | — | 성공 여부 검증 |
| 커밋 훅 | Black · isort · Ruff | — |

백엔드 CI는 **PostgreSQL 16 컨테이너를 붙여서** 돌린다. DB가 없으면 통합 테스트 **21개 파일**이 조용히 건너뛰어져, 초록불인데 사실은 안 돈 상태가 되기 때문이다.

### 전수 점검을 한 번 돌린 결과

12개 도메인으로 나눠 코드 전체를 훑었다.

| 심각도 | 발견 | 처리 |
|---|:---:|:---:|
| CRITICAL | 2 | **2** |
| HIGH | 9 | **9** |
| MEDIUM | 18 | **16** |
| LOW | 10 | 3 |

이 과정에서 테스트가 **253건 → 344건**으로 늘었다. 잡은 것 중에는 refresh 토큰 쿠키에 httpOnly가 빠져 있던 XSS 경로, `X-Forwarded-For` 첫 값을 믿어 IP를 위조하면 요청 제한을 우회할 수 있던 문제가 있다.

**남긴 항목과 남긴 이유도 같은 문서에 적었다.** 실제 피해가 없거나, 운영 지표를 본 뒤에 판단하기로 한 것들이다.

### 규모

<div align="center">

| | 백엔드 | 프론트엔드 |
|---|:---:|:---:|
| 코드 | **19,474** LOC · 145 files | **55,853** LOC · 280 files |
| 구조 | 엔드포인트 116 · 도메인 15 | 페이지 43 · 컴포넌트 38 · 컴포저블 22 |
| BFF | — | 프록시 핸들러 **120** |
| 테스트 | pytest **44** files · **344** cases | Vitest **19** files · **284** cases |
| DB | 24 tables · 64 migrations | — |

</div>

<br/>

## 확장 시 재방문 지점

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Pipe-Order/.github/main/profile/assets/scaleout-dark.png">
  <img src="https://raw.githubusercontent.com/Pipe-Order/.github/main/profile/assets/scaleout-light.png" alt="EC2 1대에서 N대로 확장할 때 깨지는 지점과 이미 안전한 지점" width="100%">
</picture>

지금은 **EC2 1대**다. nginx(443) → Nuxt(:3000) · FastAPI(:8000) → docker PostgreSQL 16.

uvicorn worker는 **일부러 1개**로 두었다. 요청 제한 카운터가 프로세스 메모리에 있어서, worker를 늘리는 순간 카운터가 갈라지기 때문이다.

실측 성능은 `/ready` 기준 **1,042 req/s · p95 37.5 ms**(동시성 40, 실패 0). 동시성을 80까지 올려 스레드풀 상한을 넘겨도 지연만 늘고 실패는 없었다.

지금 필요 없는 것은 만들지 않았다. 대신 **어디까지가 이 전제 위에 서 있는지**를 적어 뒀다.

### 서버를 2대로 늘리는 순간 깨지는 것

| 무엇이 | 왜 | 사용자가 겪는 일 |
|---|---|---|
| **요청 제한** | 카운터가 서버마다 따로 존재 | 실제 허용량이 **서버 수만큼 배**가 된다. 로그인 시도 제한도 우회된다 |
| **참조 캐시** | 무효화가 다른 서버에 전달되지 않음 | 단가를 바꿔도 **최대 5분간** 서버마다 다른 금액이 보인다 |
| **서킷 브레이커** | 차단 상태가 서버마다 독립 | 1대가 막아도 나머지는 계속 호출한다. 차단 효과가 **1/N**로 희석 |
| **토큰 갱신 병합** | Nuxt 프로세스 메모리에 존재 | 두 요청이 다른 서버로 가면 **가끔 로그아웃**된다 |
| **업로드 파일** | 올라간 서버의 로컬 디스크에만 존재 | 사용자의 **(N-1)/N**이 깨진 이미지를 본다 |
| **DB 커넥션** | 서버당 최대 40개 | **3대면 120개**로, PostgreSQL 기본 상한 100을 넘겨 연결이 거부된다 |

<details>
<summary>운영 레벨에서 추가로 볼 것 5가지</summary>

<br/>

| 무엇이 | 왜 | 대응 |
|---|---|---|
| 클라이언트 IP 판별 | 지금은 nginx 단일 홉 전제. ALB가 앞에 붙어 홉이 늘면 클라이언트가 심은 값을 믿게 된다 | 신뢰 프록시 홉 수를 고정하고 오른쪽부터 카운트 |
| 배치 스크립트 | 이미지에 crontab이 딸려 가면 N대에서 동시 실행된다 | 배치 전용 인스턴스로 분리 |
| DB 마이그레이션 | 롤링 배포로 여러 대가 동시에 스키마를 올리면 경합한다 | 배포 파이프라인에서 1회만 실행 |
| 서버 시각 | TOTP 허용 오차가 ±30초. 시계가 어긋나면 특정 서버에서만 2FA가 실패한다 | NTP를 이미지 수준에서 강제 |
| 로그 | 한 요청의 흔적이 서버 경계에서 끊긴다 | JSON 구조화 로그 + 중앙 집계 |

</details>

### 반대로, 이미 안전한 것

| | 왜 |
|---|---|
| **편집 락** | 메모리가 아니라 DB 컬럼 + 조건부 UPDATE. 서버가 몇 대든 DB가 유일한 판정자다 |
| **결제 중복 방지** | DB 인덱스와 행 잠금이 판정한다. 어느 서버가 요청을 받아도 결과가 같다 |
| **로그인 세션** | JWT + DB에 저장한 해시. 세션 서버가 따로 필요 없다 |

요청 제한 · 캐시 · 서킷 브레이커 세 모듈은 처음부터 `get / set / invalidate` 인터페이스 뒤에만 두었다. Redis로 옮길 때 **바꿀 곳은 어댑터 3개**고, 호출하는 쪽 코드는 건드리지 않는다.

### 증설 순서

| 시점 | 할 일 |
|---|---|
| **증설 직전** | 요청 제한 · 캐시 · 서킷 브레이커를 Redis로 옮긴다 |
| **증설 시점** | `STORAGE_TYPE=s3`로 전환하고 DB는 RDS Multi-AZ로. 이때 **커넥션 풀 총합과 `max_connections`를 먼저 맞춘다.** 로드밸런서 헬스체크는 DB까지 확인하는 `/ready` 사용 |
| **증설 이후** | 로그를 JSON으로 바꿔 중앙에서 모아 본다. 메트릭 수집과 초대형 도면 견적의 비동기 처리는 **실측 병목을 확인한 뒤에** 착수 |

증설 판단은 위 실측치를 기준선으로 삼아, p95 지연과 DB 커넥션 사용률로 정한다.

<br/>

## 남은 작업

운영에 필요한 코드는 들어가 있고 외부 계약이나 키 발급을 기다리는 항목들이다.

- Sentry — 연동 코드 완료(`send_default_pii=false`), DSN 발급 대기
- Cloudflare Turnstile — 프론트 · 백엔드 통합 완료, 키가 비면 양쪽 모두 자동 비활성
- 메일 — `MAIL_BACKEND=console` 상태. SMTP 설정만 채우면 코드 변경 없이 실발송 전환
- 휴대폰 본인인증(PASS/NICE), 세금계산서 발행 — 연동 계약 필요, 현재는 관리자 수동 처리
- 백업 오프사이트 사본 — 현재 백업이 같은 인스턴스에 있어 인스턴스와 함께 죽는다

<br/>

<div align="center">

<sub>기획 · 설계 · 개발 · 운영 단독 수행 &nbsp;·&nbsp; 2026.03 ~ </sub>

</div>
