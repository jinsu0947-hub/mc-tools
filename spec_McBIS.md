spec.md
1. 프로젝트 개요
이름: McBIS / McRAD 통합 시스템
목적: McRAD의 도로 정보를 기반으로 도시 토지 구조를 생성하고, 템플릿 기반으로 건물을 생성
핵심 개념:
road 기반 도시 생성
parcel 기반 건물 생성
superparcel 기반 단지 구성
blueprint / footprint 분리
템플릿 기반 건물 생성
2. 시스템 구조 (확정된 구조만)
구성 요소:
road_parcel
→ block
→ parcel
→ superparcel
→ building
관계:
road_parcel → block 생성
block → parcel 분할
parcel → building 생성 단위
parcel 집합 → superparcel
superparcel → 단지 단위
3. 데이터 구조 (명시된 것만)
entity: 공통 변수
id
parent_id
x
y
z
layer
category
entity: 공통 파일
blocks.json
parcels.json
buildings.json
roads.json
terrain.json
zoning.json
metadata.json
analysis/
entity: superparcel
parcel_ids[]
4. 핵심 로직 (이미 결정된 규칙만)
규칙 1:
도로는 McRAD에서 불러온 정보를 기반으로 생성되며, 임의로 생성되지 않는다
규칙 2:
road_parcel → block → parcel → superparcel 순서로 도시 토지 구조를 생성한다
규칙 3:
parcel은 건물 생성 단위다
규칙 4:
superparcel은 parcel의 집합으로 정의된다
규칙 5:
superparcel 영역은 parcel 영역의 합으로 구성된다
규칙 6:
superparcel은 사용자가 parcel을 선택하여 생성한다
규칙 7:
모든 공간은 parcel로 표현된다
5. 설계 결정 사항 (“왜 이렇게 했는지” 포함)
결정 1:
도로를 별도 생성 객체가 아닌 지번을 가진 토지 객체로 정의
→ 도로도 parcel과 동일한 토지 단위로 취급하기 위함
결정 2:
superparcel을 parcel의 집합으로 정의
→ 단지 개념을 건물 유형과 분리하기 위함
결정 3:
superparcel 생성은 자동이 아니라 사용자 선택
→ 건물 유형과 단지 구성이 일치하지 않기 때문
결정 4:
site_layout 제거
→ superparcel이 parcel의 집합이므로 잔여 공간이 존재하지 않기 때문
6. 제약 조건 (반드시 지켜야 하는 것)
도로는 임의 생성되지 않는다
superparcel은 parcel로만 구성된다
superparcel 내부에 잔여 공간이 존재하지 않는다
모든 토지는 parcel 단위로 표현된다
7. 미확정 / 논의 중 (중요: 절대 채우지 말 것)
없음
8. 제외된 아이디어 (논의됐지만 채택 안 된 것)
site_layout 도입
development pattern 자동 적용
parcel 자동 묶기

[FINAL CHECK]

내가 추가한 내용이 있는가? → 없음
추측한 내용이 있는가? → 없음
