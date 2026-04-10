spec.md
1. 프로젝트 개요
이름: Procedural Road Network Generation System
목적: 사용자 정의 도로 노드와 연결 관계를 기반으로 도로 네트워크를 생성하고 교차로 및 차선 연결 구조를 자동으로 계산하며, 이후 도시 생성 시스템과 연동 가능한 도로 데이터를 생성
핵심 개념:
Node 기반 도로 그래프 구조
구조 데이터와 기하 데이터의 분리
차선 중심선은 교차로 계산 시에만 생성
교차로는 그래프 패턴으로 정의
geometry는 파생 데이터로 생성
2. 시스템 구조

(확정된 구조만)

구성 요소:
Node
Road Segment
Centerline (Polyline, System Vertex 포함)
Intersection
Lane (파생 구조)
Lane Node
Lane Connection
Turn Curve
관계:
Node → Road Segment 연결
Road Segment → centerline 생성
Node (3개 이상 연결) → Intersection 생성
Intersection → Boundary 생성
Boundary → Lane Node 생성
Lane Node → Lane Connection 생성
Lane Connection → Turn Curve 생성
Lane은 Road Segment에서 offset으로 계산되는 파생 구조
3. 데이터 구조

(명시된 것만)

Node:
fields:
node_id
x
y
Road Segment:
fields:
road_id
start_node
end_node
lane_count
lane_width
road_width
direction_type
Centerline:
fields:
polyline vertices (x, y)
Intersection (선택):
fields:
intersection_id
node_id
boundary_polygon
4. 핵심 로직

(이미 결정된 규칙만)

규칙 1: 도로 네트워크는 Node–Road Segment 그래프 구조로 구성된다
규칙 2: 교차로는 3개 이상의 Road Segment가 연결된 Node에서 자동 생성된다
규칙 3: Road Segment는 하나의 객체로 저장되며 차선은 별도 객체로 저장되지 않는다
규칙 4: 차선 중심선은 centerline offset으로 계산되며 교차로 계산 시에만 생성된다
규칙 5: centerline은 polyline 기반으로 표현되며 System Vertex는 자동 생성된다
규칙 6: 곡선 도로의 접선은 Road Segment가 아닌 Node에서 결정된다
규칙 7: 교차로 내부 구조는 lane-to-lane 연결 방식으로 생성된다
규칙 8: Lane Node는 교차로 boundary 위에 생성된다
규칙 9: geometry는 구조 데이터를 기반으로 계산되는 파생 데이터이다
규칙 10: 도로 표면 생성은 centerline → lane boundary → road polygon → rasterization 순서로 수행된다
5. 설계 결정 사항

(“왜 이렇게 했는지” 포함)

결정 1: Road Segment를 하나의 객체로 저장
이유: 데이터 구조 단순화 및 lane 수 변경 시 재계산 가능하도록 하기 위함
결정 2: lane을 저장하지 않고 필요 시 생성
이유: 메모리 절약 및 계산 구조 단순화
결정 3: centerline을 polyline으로 표현
이유: rasterization과 offset 계산의 안정성 확보
결정 4: vertex는 시스템이 자동 생성
이유: 곡선 표현 품질 확보 및 사용자 편집 단순화
결정 5: tangent는 Node 기준으로 결정
이유: 교차로에서 도로 연결의 연속성과 안정성 확보
결정 6: lane centerline은 교차로 계산 시에만 생성
이유: 성능 최적화 및 불필요한 데이터 생성 방지
결정 7: Export는 centerline 기반으로 저장하고 polygon은 계산 단계에서 생성
이유: 데이터 손실 방지 및 확장성 확보
결정 8: 블록 분할은 road polygon 기반으로 수행
이유: 블록 경계 계산의 명확성과 안정성 확보
결정 9: 시스템을 파일 기반으로 분리
이유: 모듈 독립성, 확장성, 디버깅 용이성 확보
6. 제약 조건

(반드시 지켜야 하는 것)

Node는 도로 제어점이며 교차로 자체가 아니다
교차로는 Node가 아닌 구조 패턴으로 정의된다
lane은 저장되지 않는 파생 구조이다
geometry는 항상 구조 데이터로부터 계산된다
Road Segment는 lane 단위로 분리 저장되지 않는다
lane centerline은 교차로 계산 시에만 생성된다
Node 근처에는 geometry 생성을 제한하는 buffer 개념이 존재한다
교차로 내부 연결은 lane-to-lane 구조를 따른다
7. 미확정 / 논의 중

(중요: 절대 채우지 말 것)

centerline을 언제 생성할지에 대한 두 방식 비교 (road 생성 시 vs 교차로 계산 시) → 교차로 계산 시 생성으로 선택되었으나 구조적 선택지로 언급됨
vertex 생성 주체 (사용자 vs 시스템) → 시스템 자동 생성으로 결정되었으나 UI 관점 선택지로 언급됨
교차로 boundary 크기 계산 공식의 구체 수식
lane connection 충돌 해결 알고리즘의 구체 방식
block detection 알고리즘 구체 방식
8. 제외된 아이디어

(논의됐지만 채택 안 된 것)

spline 기반 centerline 저장
lane을 독립 객체로 저장하는 구조
road polygon만을 저장하는 데이터 구조
Road Segment를 lane 단위로 분리하여 저장하는 방식

================================================

spec.md
1. 프로젝트 개요
이름: Minecraft 도시 도로 설계 툴 (McRAD)
목적: Minecraft 기반 도시 서버에서 대규모 도로망을 사실적이면서도 효율적으로 설계하고, Schematic 형태로 내보낼 수 있는 벡터 기반 도로 편집 툴
핵심 개념:
자동 생성 + 사용자 조작
재계산형 엔진(B UX)
벡터 중심 설계 → Raster 변환
Node 기반 도로 그래프 구조
Lane 중심 교통 구조 (교차로 내부)
Graph와 Geometry의 분리
2. 시스템 구조

(확정된 구조만)

구성 요소:
Node
Road Segment
Lane (교차로 내부)
Lane Connection
Intersection
Turn Curve
Geometry (centerline, offset, boundary)
Rasterization 결과
관계:
Node → Road Segment
Road Segment → Centerline
Centerline → Lane 생성 (offset 기반)
교차로에서:
Road Segment → Lane Segment → Lane Connection → Turn Curve
Geometry는 Structure 데이터를 기반으로 생성되는 파생 데이터
Graph 구조와 Geometry 구조는 분리됨
3. 데이터 구조

(명시된 것만)

Node:
fields:
id
x, y, z
connected_segments
node_type
Road Segment:
fields:
id
start_node
end_node
centerline_geometry
lane_count
lane_width
road_width
direction
attributes
Lane (교차로 내부):
fields:
id
parent_segment_id
lane_index
start_lane_node
end_lane_node
lane_centerline
turn_type
Lane Connection:
fields:
from_lane_id
to_lane_id
turn_type
active
Intersection:
fields:
id
center_node_id
boundary_polygon
lane_nodes
min_radius
safety_margin
Turn Curve:
fields:
id
start_lane_node
end_lane_node
curve_type
control_points
sample_points
Geometry / Rasterization:
fields:
lane_centerline_samples
lane_boundary
road_polygon
rasterized_blocks
4. 핵심 로직

(이미 결정된 규칙만)

규칙 1:
Node 기반으로 Road Segment를 생성한다.
Active Node를 기준으로 새로운 Node 생성 시 자동 연결된다.
규칙 2:
기존 Node 클릭 시 Active Node가 변경된다.
규칙 3:
Node 간 선택으로 임의 연결이 가능하다.
규칙 4:
교차로는 3개 이상의 Road Segment가 연결된 Node에서 생성된다.
규칙 5:
교차로 경계는 중심 교차점에서 일정 거리 후퇴하여 생성한다.
규칙 6:
Lane은 centerline + offset으로 계산된다.
규칙 7:
Lane은 교차로 내부에서만 객체로 생성된다.
규칙 8:
Lane Connection은 incoming lane → outgoing lane 구조로 생성된다.
규칙 9:
Lane 연결은 방향 벡터 기반으로 직진 / 좌회전 / 우회전 / U턴으로 분류된다.
규칙 10:
Turn Curve는 lane 중심선 기반으로 생성된다.
규칙 11:
곡선은 Arc 기반이며, 필요 시 Bezier로 변환된다.
규칙 12:
Geometry는 Structure 데이터로부터 계산되는 파생 데이터이다.
규칙 13:
Rasterization은 vector → pixel 변환 과정에서 수행된다.
규칙 14:
1px = 1 Minecraft block
규칙 15:
Pixel Line Smoothing은 rasterize 이후 수행된다.
규칙 16:
Parallel Lane 처리 시 최소 픽셀 간격 유지
규칙 17:
연결 규칙(연속성 유지)이 픽셀 표현보다 우선한다.
5. 설계 결정 사항

(“왜 이렇게 했는지” 포함)

결정 1:
Lane을 항상 저장하지 않고 centerline offset으로 계산
이유: 메모리 절약 및 구조 단순화
결정 2:
Lane은 교차로 내부에서만 분리
이유: lane 연결 및 규칙 처리는 교차로에서만 필요
결정 3:
Graph 구조와 Geometry 구조 분리
이유: 수정 시 재계산 용이 및 데이터 일관성 유지
결정 4:
Arc 곡선을 기본으로 사용
이유: 계산 단순성과 안정성
결정 5:
필요 시 Arc → Bezier 변환 허용
이유: 사용자 의도 반영
결정 6:
vector → bitmap → schematic 구조 채택
이유: Minecraft 환경 대응
결정 7:
float 좌표 사용 후 rasterization 시 integer 변환
이유: 곡선 및 offset 정확도 확보
결정 8:
Active Node 기반 인터랙션
이유: 연속 도로 생성 및 UX 단순화
결정 9:
연결 규칙 우선 처리
이유: 그래프 구조 유지가 시각적 표현보다 중요
6. 제약 조건

(반드시 지켜야 하는 것)

모든 좌표는 float 기반으로 계산 후 최종 출력에서만 integer 변환
모든 객체는 ID 기반으로 참조
Graph와 Geometry는 반드시 분리
Lane은 교차로 내부에서만 생성
1px = 1 block 유지
vector 기반 계산 후 rasterization 수행
연결 구조는 항상 유지되어야 함
7. 미확정 / 논의 중

(중요: 절대 채우지 말 것)

교차로 생성 규칙의 상세 로직
회전교차로 / 로터리 처리 방식
교차로 내부 lane 연결 세부 규칙
추가 설계 일부 요소
8. 제외된 아이디어

(논의됐지만 채택 안 된 것)

Lane을 항상 독립 객체로 저장하는 구조
