## Abstract

FAST LIO2라는 빠르고 견고한 다목적 LiDAR-Inertial(관성) Odometry Framework를 제안한다. 

이 System은 고효율의 밀접 결합된 반복 Kalman Fileter를 기반으로 두 가지 주요 혁신 요소를 포함한다. 

1. Feature 추출 없이 Raw Point를 지도에 직접 등록해 Map을 Update 하는 방식이다. 
이는 환경의 Subtle(세부) Feature를 활용해 정확도를 높이고 수작업으로 설계된 Feature 추출 Module을 제거해 다양한 LiDAR Scan Pattern에 자연스럽게 적용한다. 
2. ikd-Tree라고 불리는 Incremental k-d Tree Data 구조를 통해 Map을 유지 관리하는 방식이다.
Point 삽입, 삭제, 동적 Re-Balancing과 같은 Incremental Update를 지원한다. 
이는 기존의 동적 Data 구조보다 전반적인 성능이 우수하며 Tree 내에서 Down-Sampling을 지원한다. 

다양한 Public LiDAR Dataset에서 19개의 Sequence를 Benchmark 비교한 결과 FAST LIO2는 낮은 계산 부하에서 지속적으로 높은 정확도를 보였다. 

소형 FoV(Field of View)를 가진 LiDAR에서도 Reliable Pose Estimation(견고한 자세 추정)을 실현하고, UAV, Handheld Platform 등 다양한 Platform에서 높은 정확도를 유지한다. 

## Conclusion

FAST LIO2는 빠르고 견고한 LiDAR-Inertial Odometry Framework로 기존의 최신 기술보다 더 높은 효율성과 정확도를 보여준다. 

Feature Extraction Module을 제거하고 효율적인 Mapping 방식을 채택하여 속도를 대폭 개선했으며, 새로운 ikd-Tree Data 구조는 Realtime Mapping 작업에 탁월한 성능을 제공한다. 

여러 실험에서 FAST LIO2는 가장 높은 정확도와 낮은 계산 부하를 유지하며 급격한 동작이나 Point 밀도가 낮은 환경에서도 높은 안정성과 정확도를 입증했다. 
또한 FAST LIO2는 다양한 LiDAR Sensor에 자연스럽게 적응할 수 있어 기존 방식보다 널리 적용될 수 있는 가능성을 보여준다. 

## Results & Discussion

### Data & Benchmark Result

1. ikd-Tree 성능 비교 

- 기존의 R*-Tree, Octree, nanoflann k-d Tree보다 k-NN 검색과 Point 삽입에서 더 높은 효율성을 보인다.
- ikd-Tree는 Log 복잡도의 삽입 및 검색을 지원하며 Incremental Update를 통해 Realtime 작업에서 발생할 수 있는 지연을 줄인다.
1. 정확도 Benchmark
    - 총 19개의 Dataset에서 RMSE(평균 제곱근 오차) 및 Drift를 비교한다.
    - FAST LIO2는 대다수 Sequence에서 기존 방법(LILI-OM, LIO-SAM, LINS)를 능가하며 가장 낮은 오차를 기록한다.
2. 처리 시간 분석
    - FAST LIO는 평균적으로 초당 10-100Hz의 속도로 LiDAR Data를 처리하며 다른 최신 기술보다 10배 빠른 속도를 기록했다.

### Detail

1. ikd-Tree 구조 및 구현
    - Point 삽입, 삭제 및 범위 검색 기능을 효율적으로 지원한다.
    - 병렬 Rebuilding을 통해 Realtime 작업 중 발생할 수 있는 지연을 방지한다.
2. 실험 Platform
    - UAV, Handheld 및 GPS Navigation Platform 등 다양한 환경에서 Test
    - 빠른 움직임과 제한된 FoV에서도 높은 정확도를 유지한다.
3. Realtime Mapping
    - 대규모 실내외 환경에서 소형 LiDAR로 3D Map 생성
    - UAV를 이용한 공중 Mapping에서 정확하고 세밀한 구조 Capture

## Introduction

Realtime 3D Map 구축 및 SLAM(Map 기반 위치 추정)은 자율 Robot이 안전하게 미지의 환경을 탐색하기 위해 필수적이다. 

그러나 시각 기반 SLAM은 조명 변화와 Motion Blur에 취약하며 밀도가 낮은 Feature Map만 유지한다. 

반면 LiDAR Sensor는 정확하고 조밀한 Depth 정보를 제공하여 최근 자율주행 자동차와 UAV와 같은 많은 자율 Robot에서 중요한 역할을 한다. 

이러한 LiDAR 기반 SLAM은 제한된 계산 자원에서 효율적이고 정확한 상태 추정과 밀도 높은 3D Map을 요구한다. 
하지만 기존의 LiDAR 방식은 높은 계산 부하와 환경에 따라 달라지는 Featrue Extraction 성능으로 인해 한계가 있다. 

## Expertise

### LIO Algorithm

LOAM은 **LiDAR Data를 기반으로 Pose Estimation과 3D Map 생성을 동시에 수행하는 Algorithm**이다. 

Edge, 평면 같은 Feature를 Extraction하여 Mapping과 Pose Estimation을 수행하지만 계산 부하가 높고 Feature가 적은 장면에서는 성능이 저하될 수 있다. 

LIO는 **LiDAR와 IMU를 결합**해 Pose Estimation을 수행하는데, 이때 LiDAR는 주변 환경의 Depth Data를 제공하며 IMU는 가속도 및 각속도에 Data를 측정해 빠르고 정확한 Pose Estimation을 돕는다. 

LIO는 LiDAR의 고정밀 Data와 IMU의 Realtime 동작 Data를 결합해 높은 정확도를 제공한다. 

### idk-Tree

ikd-Tree는 기존의 kd-Tree를 개선한 **Incremental Data** 구조이다. 

1. **효율적인 Data 삽입 및 삭제**
    - 기존 kd-Tree는 새로운 Data 삽입 시 전체 Tree Rebuilding 해야 했지만 ikd-Tree는 필요한 부분 Update하여 Realtime 처리를 지원한다.
2. **동작 Re-Balancing**
    - Data 삽입, 삭제로 인해 발생하는 Tree의 불균형을 Realtime으로 조정한다.
3. **kNN 검색 속도 개선**
    - ikd-Tree는 k-Nearest Neighbor 검색 속도를 기존 kd-Tree 대비 약 4%의 시간만으로 수행할 수 있어 대규모 Point Cloud Data 처리에 적합하다.

### FAST LIO & FAST LIO2

| FAST LIO | FAST LIO2 |
| --- | --- |
| **LOAM Based Feature Extraction** <br> Lidar Odometry and Mapping 기반 Feature Extraction 방식을 사용한다. 특정한 Point(Edge, 평면)을 추출해 Mapping, Pose를 Estimation  | **Direct Point Registration** <br> LOAM 기반 Feature Extraction을 제거, Point Cloud를 직접 정합하는 방식(Direct Registration) 방식으로 전환한다. 이는 Feature Extraction 과정의 불필요한 계산을 제거, LiDAR Sensor에서 높은 성능을 제공할 수 있다.  |
| **Forward, Backward Propagation** <br> Data의 Forward, Backward Propagation(전파) 방식을 사용해 상태를 예측하고 보정한다.  | **Forward, Backward Propagation** <br> Data의 Forward, Backward Propagation(전파) 방식을 사용해 상태를 예측하고 보정한다.  |
| **Iterated Kalman Filter** <br> Iterated Kalman Filter를 사용해 Pose Estimation 정확도를 높인다.  | **Iterated Kalman Filter** <br> Iterated Kalman Filter를 사용해 Pose Estimation 정확도를 높인다.  |
| **Efficient Kalman Gain** <br> 계산 효율성을 높이기 위해 최적화 된 Kalman Gain을 사용한다.  | **Efficient Kalman Gain** <br> 계산 효율성을 높이기 위해 최적화 된 Kalman Gain을 사용한다.  |
| **kd-Tree Based Mapping** <br> kd-Tree 구조를 사용해 Point Cloud Data를 관리하고 Mapping 한다.  | **ikd-Tree Mapping** <br> kd-Tree를 대신해 ikd-Tree 구조를 사용, Realtime Data 삽입, 삭제, Re-Balancing을 통해 속도를 대폭 개선하며 kNN 검색 속도를 크게 향상시켰다.  |
1. **LOAM Feature Based → Direct Point Registration로 Odometry 방식 변화** 
    - LOAM Feature Based : Point 주변의 Curvature(곡률)를 측정해 Edge, 평면 Feature Point으로 분류
    - Direct Point Registration : ikd-Tree의 빠른 검색 속도로 인해 Feature Extraction 없이 바로 Point Registration(등록)
2.  **kd-Tree Mapping → ikd-Tree Mapping** 
    - Incremental kd-Tree : 대규모 Point Cloud를 효율적으로 삽입, 삭제, 탐색할 수 있는 자료구조로 기존 정적 kd-Tree의 한계와 수동 Re-Balancing 시 모든 Tree를 변형하는 문제를 부분 Tree Update로 변경해 속도를 증가시켰다.