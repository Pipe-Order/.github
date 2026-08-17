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

## 기술적으로 공들인 지점

<details>
<summary><b>비즈니스 불변식은 애플리케이션이 아니라 DB가 강제한다</b></summary>

<br/>

"취소되지 않은 주문은 도면당 1건", "주문당 활성 결제는 1건". 이런 규칙은 서비스 코드의 일이 아니라 **부분 유니크 인덱스**의 일이다.

```sql
CREATE UNIQUE INDEX ux_orders_active_snapshot_id ON orders (snapshot_id)
  WHERE snapshot_id IS NOT NULL AND status != 'CANCELLED';

CREATE UNIQUE INDEX ux_payments_active_order_id ON payments (order_id)
  WHERE status NOT IN ('CANCELED', 'EXPIRED', 'FAILED');
```

애플리케이션 선검사는 **친절한 에러 메시지(UX)** 를 위한 것이고, 진짜 방어선은 DB 제약이다. 선검사를 동시에 통과한 두 요청 중 하나는 반드시 `IntegrityError`가 나며, 이걸 500이 아니라 **409 Conflict**로 변환한다.

현재 부분 유니크 인덱스 4개, CHECK 제약 10개가 걸려 있다. 그중 5개(`orders.status`, `payments.status`, `project_members.role`, `snapshots.mode`, `part_specs.part_type`)는 모델이 아니라 마이그레이션에만 선언했다. enum 값을 native PG ENUM 대신 varchar로 두면 값 추가 때 `ALTER TYPE`이 필요 없어 유연하고, 무결성은 CHECK로 따로 보강하면 되기 때문이다. 어떤 컬럼에 걸고 어떤 컬럼(`companies.plan` 같은 자유 문자열)에는 걸지 않을지 기준을 마이그레이션 docstring에 남겼다.

</details>

<details>
<summary><b>토큰 로테이션과 가용성의 트레이드오프 — 8초짜리 유예 창</b></summary>

<br/>

다중 탭에서 동시에 refresh를 호출하면 근거 없이 로그아웃되는 버그가 있었다. 먼저 커밋한 쪽이 토큰 값을 바꿔버려, 뒤늦게 행 잠금을 얻은 요청의 `WHERE` 조건이 성립하지 않았기 때문이다.

> 행 잠금은 *순서*를 보장할 뿐 *조건의 유효성*을 보장하지 않는다.

직전 토큰 해시와 로테이션 시각을 남기고, **8초 유예 창** 안에 도착한 구 토큰은 재로테이션 경로를 타게 해 해결했다. 유예 창 밖의 재사용은 여전히 401이므로 탈취 방어는 그대로다.

프론트엔드에도 같은 문제의 다른 층이 있었다. 클라이언트는 httpOnly 쿠키를 읽을 수 없어 탭 간 조율에 쓸 공유 상태가 없다. 그래서 같은 탭의 동시 401은 모듈 전역 `refreshPromise` 하나로 합치고, 여러 탭이 동시에 보낸 갱신 요청은 Nuxt 서버가 in-flight Map으로 백엔드 호출 1회로 합친다. 방금 회전된 옛 토큰을 든 요청은 60초 grace 창에서 새 토큰을 재사용한다. 다른 기기 로그인으로 세션이 무효화된 경우(`SESSION_REVOKED`)는 재시도 없이 즉시 로그아웃한다.

</details>

<details>
<summary><b>결제 웹훅의 본문을 믿지 않는다, 그리고 "실패"와 "판정 불가"를 구분한다</b></summary>

<br/>

웹훅 페이로드는 누구나 만들어 보낼 수 있다. 그래서 본문에서는 `paymentKey`만 꺼내고, 금액과 상태는 **Toss API에 다시 조회한 결과만** 사실로 채택한다. 재조회는 우리 시크릿 키로만 가능하므로 위조된 페이로드가 주문 상태를 바꿀 수 없다. 서명 검증 대신 택한 구조이고, 그렇게 판단한 근거를 코드 주석에 남겼다.

재시도는 **멱등성 기준**으로 갈랐다. 조회(GET)는 1회 재시도하고, **결제 승인(POST)은 재시도하지 않는다.** 타임아웃 뒤에도 서버 쪽에서는 성공했을 수 있어 이중 승인 위험이 있기 때문이다. 최종 상태는 웹훅이 수렴시킨다.

네트워크 타임아웃은 결제 실패가 아니다. 그래서 `FAILED`로 마킹하지 않고 503으로 응답한다. **"실패"와 "판정 불가"를 구분하지 않으면 정상 결제를 유실한다.** 반대로 DONE이었던 결제가 CANCELED로 돌아오면 환불로 간주해 주문을 자동 취소하지 않고 `PAUSED`로 세워, 사람이 판단할 여지를 남긴다.

외부 호출에는 연속 5회 실패 시 30초간 회로를 여는 서킷 브레이커를 걸었다(Toss, 국세청 공통). 외부 장애 시 요청 스레드가 타임아웃까지 붙잡히면 스레드풀이 고갈되어 서비스 전체가 멈추기 때문이다.

</details>

<details>
<summary><b>인가 누락이 구조적으로 불가능한 설계</b></summary>

<br/>

경로 파라미터로 리소스 ID를 받는 엔드포인트는 반드시 "리소스 로딩 + 인가"를 한 몸으로 묶은 FastAPI 의존성을 사용한다.

```python
@router.post("/{project_id}/save-canvas")
def save_canvas(project: Project = Depends(get_editable_project), ...):
```

인가 호출을 "잊는" 실수가 **함수 시그니처 차원에서 차단**된다. 인가 로직을 통과하지 않으면 애초에 `project` 객체를 손에 넣을 수 없기 때문이다. 역할 × 리소스 조합은 인가 매트릭스 테스트로 전수 검증한다(프로젝트 15케이스, 회사 멤버 13케이스).

</details>

<details>
<summary><b>하이브리드 저장 전략 — JSONB와 정규화를 언제 나누는가</b></summary>

<br/>

**형상 데이터는 JSONB.** 부품 타입마다 필요한 필드가 다르다(엘보엔 각도, 티엔 분기경, 리듀서엔 양쪽 외경). 정규화하면 NULL 컬럼이 폭증하는 EAV 안티패턴에 빠지고, 새 부품 타입을 추가할 때마다 마이그레이션이 붙는다. 조회 단위가 항상 "프로젝트 전체"라 조인이 안 된다는 단점이 문제가 되지 않는다. 현재 8개 테이블 18개 컬럼이 JSONB다.

**규격 · 단가 카탈로그는 정규화.** 조인 · 집계가 필요하고 무결성이 금액과 직결된다. 파이프 단가와 부속 단가가 한 테이블에 섞이지 않도록 배타적 FK를 CHECK로 강제했다.

```python
CheckConstraint(
    "(pipe_spec_id IS NOT NULL AND part_spec_id IS NULL) OR "
    "(pipe_spec_id IS NULL AND part_spec_id IS NOT NULL)",
    name="check_exclusive_spec_fk",
)
```

같은 기준을 삭제 정책에도 적용했다. 대부분의 FK는 CASCADE나 SET NULL이지만 `payments.order_id` 하나만 RESTRICT다. 돈이 오간 기록은 참조 무결성 편의보다 보존이 우선이라고 판단해, 한 번 CASCADE로 바꿨던 것을 되돌렸다.

</details>

<details>
<summary><b>금액 버그는 테스트로 고정한다</b></summary>

<br/>

용접비 누진 계산이 구간별로 `int()` 절삭을 쓰고 있었다. 소수부가 늘 버려지는 방향이라 청구액이 조금씩 적게 나왔다. 금액 오류는 한 번 나가면 되돌리기 어려우므로, 원 단위 절상(`math.ceil`)으로 정책을 통일하고 구간 경계값을 회귀 테스트로 못박았다.

파이프 길이 합산에서 `null`이 아닌 첫 값만 읽어 일부 구간(500mm)이 누락되던 버그도 같은 방식으로 처리했다. 견적 엔진이 ORM을 모르는 순수 함수라, 이런 회귀 테스트는 DB도 픽스처도 없이 돈다. 현재 가격 관련 테스트만 27건이다.

</details>

<details>
<summary><b>제품 판단이 스키마에 남는다 — 광고 배너 자리 판매를 접은 이유</b></summary>

<br/>

배너 광고를 처음엔 "자리 지정"(1번 자리, 2번 자리) 방식으로 팔도록 만들었다. 문제는 그 자리를 산 광고주가 하나뿐일 때다. 돈을 받고도 특정 상황에서 광고가 노출되지 않는 구간이 생긴다.

노출 로테이션 방식으로 바꾸면서 `ad_banners.position` 컬럼을 마이그레이션으로 삭제했다. 왜 지웠는지는 마이그레이션 docstring에 남아 있다. 스키마 변경 이력이 곧 제품 판단의 이력이 되도록 유지하려 했다.

</details>

<br/>

## 코드 품질

CI는 push · PR마다 두 저장소에서 각각 돈다. 백엔드는 **postgres:16 서비스 컨테이너를 붙여** ruff lint와 pytest 전체(단위 + 통합)를 실행한다. DB 없이 돌리면 통합 테스트 21개 파일이 조용히 skip돼 "초록불인데 사실 안 돈 상태"가 되기 때문이다. 프론트엔드는 eslint · `vue-tsc` 타입체크 · vitest에 더해 **프로덕션 빌드 성공 여부**까지 별도 job으로 검증한다. 커밋 단계에서는 pre-commit(Black · isort · Ruff)이 스타일과 정적 오류를 막는다.

2026년 8월, 12개 도메인을 나눠 정적 코드 감사를 한 번 돌렸다. **총 35건 중 CRITICAL 2/2, HIGH 9/9, MEDIUM 16/18을 처리**했고, 이 과정에서 테스트가 253개에서 344개로 늘었다. 잡은 것 중에는 refresh 토큰 쿠키에 httpOnly가 빠져 있던 XSS 경로, `X-Forwarded-For`의 첫 값을 신뢰해 IP 위조로 rate limit을 우회할 수 있던 문제, 위에 적은 용접비 절삭 버그가 있다. 남긴 것과 남긴 이유(실피해가 없거나 모니터링 후 판단)도 같은 문서에 적어 뒀다.

<div align="center">

| | 백엔드 | 프론트엔드 |
|---|:---:|:---:|
| 코드 | **19,474** LOC · 145 files | **55,853** LOC · 280 files |
| 구조 | 엔드포인트 116 · 도메인 15 | 페이지 43 · 컴포넌트 38 · 컴포저블 22 |
| BFF | — | 프록시 핸들러 **120** |
| 테스트 | pytest **44** files · **344** cases | Vitest **19** files · **284** cases |
| DB | 24 tables · 64 migrations | — |

</div>

1인 개발이라 리뷰어가 없다. 그 공백을 CI 게이트, DB 제약, 인가 매트릭스 테스트, 그리고 주기적 코드 감사로 메웠다.

<br/>

## 확장 시 재방문 지점

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Pipe-Order/.github/main/profile/assets/scaleout-dark.png">
  <img src="https://raw.githubusercontent.com/Pipe-Order/.github/main/profile/assets/scaleout-light.png" alt="EC2 1대에서 N대로 확장할 때 깨지는 지점과 이미 안전한 지점" width="100%">
</picture>

현재는 EC2 한 대 위에서 nginx(443) → Nuxt(:3000) · FastAPI(:8000) → docker PostgreSQL이 돈다. uvicorn worker는 **의도적으로 1개**다. rate limiter와 캐시가 프로세스 메모리에 있어서, `--workers N`을 주는 순간 카운터가 워커마다 갈라지기 때문이다. 지금 필요하지 않은 것을 미리 만들지는 않았지만, **어디까지가 이 전제 위에 서 있는지는 좌표까지 적어 뒀다.**

### 서버를 늘리는 순간 깨지는 것

| 상태 | 위치 | EC2 N대에서 나타나는 증상 | 대응 |
|---|---|---|---|
| Rate limiter | `core/rate_limit.py` | 슬라이딩 윈도와 로그인 실패 카운터가 프로세스 dict. 실효 한도가 **N배**가 되고, 15분 15회 실패 → 10분 잠금도 우회된다 | Redis 이전 |
| 참조 캐시 | `core/cache.py` (TTL 300s) | 관리자가 단가를 바꿔도 invalidate가 다른 인스턴스로 전파되지 않는다. **최대 5분간 인스턴스마다 다른 금액**이 조회된다 | Redis + pub/sub 무효화 |
| 서킷 브레이커 | `core/circuit_breaker.py` | 회로가 인스턴스별로 독립이라 1대가 차단해도 나머지는 계속 호출한다. **차단 효과가 1/N**로 희석 | 공유 실패 카운터 |
| refresh dedupe | `server/utils/tokenRefresh.ts` | 동시 갱신 병합이 Nuxt 프로세스 로컬. 두 요청이 다른 인스턴스로 가면 로테이션 경합으로 **간헐적 강제 로그아웃**이 뜬다 | ALB sticky session, 또는 dedupe를 걷고 백엔드 8초 유예 창에 위임 |
| 업로드 파일 | `./uploads` 로컬 디스크 | 업로드한 인스턴스에서만 파일이 보인다. 사용자의 **(N-1)/N**이 깨진 이미지를 본다 | `STORAGE_TYPE=s3` (boto3 경로는 이미 구현) + presigned URL |
| DB 커넥션 | pool 20 + overflow 20 | 프로세스당 최대 40. **3대면 120으로 PostgreSQL 기본 `max_connections` 100을 넘겨** 연결 거부가 난다 | 인스턴스당 풀 축소 또는 PgBouncer, RDS 인스턴스 클래스별 상한 확인 |
| 클라이언트 IP 판별 | `X-Forwarded-For` | 지금은 nginx 단일 홉 전제. **ALB가 앞에 붙어 홉이 늘면** 클라이언트가 심은 앞쪽 값을 신뢰하게 되어 rate limit이 다시 우회된다 | 신뢰 프록시 홉 수를 고정하고 오른쪽부터 카운트 |
| 배치 스크립트 | OS crontab (`backup_db`, `purge_*`) | 이미지에 crontab이 딸려 가면 **N대에서 동시 실행**되어 백업이 중복되고 정리 작업이 서로를 밟는다 | 배치 전용 인스턴스로 분리, 또는 EventBridge + 단일 실행 보장 |
| Alembic 마이그레이션 | 배포 시 실행 | 롤링 배포로 여러 대가 동시에 `upgrade head`를 때리면 경합한다 | 배포 파이프라인에서 1회만 실행 (advisory lock) |
| 서버 시각 | TOTP `valid_window=±30초` | 인스턴스 간 시계가 드리프트하면 **특정 인스턴스로 라우팅될 때만** 관리자 2FA가 실패한다 | chrony/NTP를 AMI 수준에서 강제 |
| 로그 | 인스턴스 로컬 파일, 일 단위 로테이션 | 한 요청의 흔적이 인스턴스 경계에서 끊긴다. request_id는 있는데 모아 볼 곳이 없다 | JSON 구조화 로그 + 중앙 집계 |

### 반대로, 이미 안전한 것

- **편집 락**은 메모리가 아니라 `projects.locked_by_id` + 조건부 UPDATE다. 인스턴스가 몇 대든 DB가 유일한 판정자이므로 그대로 동작한다.
- **결제 멱등성**은 부분 유니크 인덱스와 `SELECT ... FOR UPDATE`가 보장한다. confirm과 webhook이 서로 다른 인스턴스로 들어와도 결과는 같다.
- **인증**은 JWT + DB 저장 refresh 해시로 stateless다. 세션 서버가 필요 없다.
- **캐시 · rate limit · 서킷 브레이커** 세 모듈은 처음부터 `get / set / invalidate` 인터페이스 뒤로만 접근하도록 격리해 뒀다. Redis 전환의 실제 작업 범위는 **어댑터 3개 교체**다.

### 증설 순서

1. **증설 직전 — 상태를 프로세스 밖으로.** rate limit · 캐시 · 서킷 브레이커를 ElastiCache Redis로 옮긴다. 인터페이스가 이미 분리돼 있어 호출부는 건드리지 않는다.
2. **증설 시점 — 디스크와 DB를 공유로.** `STORAGE_TYPE`을 s3로 바꾸고 CloudFront를 앞에 둔다. DB는 RDS Multi-AZ로 옮기고, 이때 커넥션 풀 총합과 `max_connections`를 먼저 맞춘다. ALB 헬스체크는 `/ready`(DB `SELECT 1` 포함)로, 프로세스 생존 확인은 `/health`로 나눠 쓴다.
3. **증설 이후 — 보이지 않으면 늘릴 수 없다.** 텍스트 로그를 JSON으로 바꾸고 request_id를 인스턴스 경계 너머로 잇는다. Prometheus 메트릭과 초대형 도면 견적의 비동기 처리는 실측 병목을 확인한 뒤에 착수한다.

현재 단일 인스턴스 실측치는 `/ready` 기준 **1,042 req/s · p50 27.4 ms · p95 37.5 ms**(동시성 40, 실패 0)다. 동시성을 80으로 올려 AnyIO 스레드풀 40을 넘겨도 지연만 늘고 실패는 없었다. 증설 판단은 이 수치를 기준선으로 삼아, p95 지연과 DB 커넥션 사용률로 정한다.

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
