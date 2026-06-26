# McBIS 명세 (spec_McBIS)
> 블록·필지·건물 모듈. **아직 착수 전.** 공통 토대는 spec_McEZR.md 참조.
> McRAD MVP 완성 후 시작 예정.

---

## 0. 상태

**준비 중.** McRAD가 우선이다. 이 문서는 잊지 않기 위한 뼈대일 뿐,
지금 채우지 않는다 (추측 금지 — spec_McEZR 원칙).

---

## 1. 역할 (spec_McEZR에서 확정된 것)

- road_parcel → block → parcel → superparcel → building 순으로 토지 구조 생성
- McRAD의 도로 데이터를 기반으로 생성 (임의 생성 금지)
- parcel = 건물 생성 단위
- superparcel = parcel의 집합 (사용자가 선택해 묶음, 잔여 공간 없음)

## 2. 확정된 데이터 (spec_McEZR 참조)

Block, Parcel, Building, Superparcel, Public/Utility Space.
상세 필드는 spec_McEZR.md 섹션 5-4 참조.

## 3. McRAD에서 재사용할 것

- 곡선 편집 모델(노드+핸들) → 필지 경계 편집에 재사용
- 좌표 변환 공통 함수
- 상위 객체가 하위를 덮는 참조 패턴 (교차로 ↔ superparcel)

## 4. 착수 시 정할 것 (지금 아님)

- 필지 자동 분할 규칙
- 건물 footprint 입력 방식
- 단면 분리 구조를 필지에 어떻게 적용할지
