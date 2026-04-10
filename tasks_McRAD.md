task.md
1. 분석 대상
Gemini 생성 단일 HTML 기반 Road Network Editor
기능 범위:
Node / Road Segment 생성
Centerline 포함
Intersection / Lane / Connection / Turn Curve
Road Polygon까지 포함
2. 핵심 문제 요약

본 코드는 다음 상태에 해당한다:

단계 기반 파이프라인 시스템이 아닌, 단일 이벤트 기반 즉시 생성 구조

이로 인해:

구조/기하 분리 실패
단계 의존성 붕괴
기능 단위 테스트 불가능
확장성 없음
3. 구조 위반 사항
3.1 단일 함수 과적재
대상
updateIntersections()
포함 기능
Road Polygon 생성
Intersection 생성
Lane Node 생성
Lane 생성
Lane Connection 생성
Turn Curve 생성
문제
기능 단위 분리 없음
모든 파생 구조를 한 번에 생성
단계별 실행 구조 위반
3.2 파이프라인 순서 붕괴
정상 구조 (spec 기준)
Node → Segment → Centerline → Intersection → Lane → Connection → Curve → Polygon
현재 구조
Node → Segment → (updateIntersections 내부에서 전부 생성)
문제
실행 순서 강제 불가
특정 단계만 실행 불가능
3.3 Structure / Geometry 분리 실패
spec 요구
Graph 구조와 Geometry 분리
현재 상태
구조	geometry
Node	centerline
Segment	lane
Intersection	polygon
	curve

→ 동일 함수에서 동시 생성

문제
재계산 구조 붕괴
일부 데이터만 갱신 불가능
4. 기능별 문제 상세
4.1 Centerline 생성 구조
현재
Segment 생성 시 즉시 포함
centerline: { polyline: [...] }
문제
Centerline 독립 생성 단계 없음
Step 5 분리 실패
4.2 Road Polygon 생성 위치 오류
현재
updateIntersections 내부에서 생성
문제
Intersection / Lane보다 먼저 생성
Geometry 후처리 단계 위반
4.3 Intersection 생성
현재
연결된 segment 수 기반 생성 (정상)
문제
생성 후 후속 구조가 즉시 결합됨
독립 구조로 유지되지 않음
4.4 Lane Node 생성
현재
position: pStart / pEnd
문제
boundary_polygon 기반 아님

spec 위반:

Lane Node는 boundary 위에 생성

4.5 Lane 생성
현재
Node 기준 방향 벡터 기반 생성
문제
centerline offset 기반 아님
진입/진출 구분 없음
방향성 없음

→ lane 정의 자체가 spec과 불일치

4.6 Lane Connection 생성
현재
for (i)
  for (j)
    if (i !== j)
문제
모든 lane 간 연결 생성

spec 위반:

incoming lane → outgoing lane

→ 잘못된 연결 포함:

역방향
U턴
비현실 연결
4.7 Turn Curve 생성
현재
p0 = fromLane 시작점
p1 = 중심 node
p2 = toLane 시작점
문제
Lane Node 기반 아님
tangent 고려 없음
중심으로 수렴하는 비정상 곡선
4.8 Lane Boundary 생성
현재
Lane 생성 시 동시에 포함
문제
Step 13 분리 실패
Geometry 단계 분리 안됨
4.9 중복 Segment 생성 가능성
현재
동일 Node 간 중복 연결 방지 없음
영향
Intersection 조건 왜곡
Lane / Connection 과다 생성
5. 데이터 흐름 문제
정상 기대 흐름
Structure
→ Geometry 생성
→ Geometry 기반 추가 구조 생성
현재 흐름
입력 이벤트
→ 전체 구조 + geometry 즉시 재생성
문제
특정 단계만 갱신 불가능
부분 디버깅 불가능
6. 상태 관리 문제
현재 상태 특징
전역 배열 기반
단일 갱신 함수
부분 업데이트 없음
문제
상태 변경 영향 범위 예측 불가
특정 데이터만 재계산 불가능
7. 설계 규칙 위반 목록
위반된 spec 규칙
규칙 4: lane centerline은 교차로 계산 시에만 생성 → 부분 위반
규칙 7: lane-to-lane 연결 → 위반
규칙 8: lane node는 boundary 위 → 위반
규칙 9: geometry는 파생 데이터 → 위반
Graph/Geometry 분리 → 위반
8. 결론

현재 코드는 다음 특성을 가진다:

기능 통합형 단일 실행 구조
단계 기반 설계 미반영
spec 대비 구조적 불일치 다수 존재
9. 상태 정의

본 코드는 다음 상태로 분류된다:

“Prototype 수준 시각화 구현”

설계 기반 시스템 아님
확장 가능한 구조 아님
유지보수/추가 개발 불가능 상태

(끝)
