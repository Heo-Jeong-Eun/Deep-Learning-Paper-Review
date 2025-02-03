## Q&A

### FAST LIO2의 주요 개선점과 기존 FAST LIO와 비교했을 때 어떤 성능 향상이 있었는가

**FAST LIO2의 주요 개선점**

1. **Direct Point Registration**
    
    LOAM 기반 특징 추출을 제거하고 Point Cloud를 직접 Map에 등록하는 방식을 도입했다. 
    
    Feature Extraction 과정에서 발생하는 계산 부하를 제거하고 다양한 LiDAR Sensor Scan Pattern에 적응성을 높였다. 
    
2. **ikd-Tree Data 구조** 
    
    기존 k-d Tree를 대체한 ikd-Tree는 Point 삽입, 삭제, Re-Balancing을 Realtime으로 지원하며 kNN Search 속도를 획기적으로 향상시켰다. 
    

**기존 FAST LIO와 비교했을 때 성능 향상**

1. **kNN 검색 및 처리 시간 단축**
    
    kNN 검색 속도가 기존 대비 약 96% 개선되었고 평균 LiDAR Scan 처리 시간이 10-100Hz로 다른 최신 기술보다 최대 10배가 빠르다. 
    
2. **정확도**
    
    대부분의 실험 Sequence에서 가장 낮은 RMSE와 Drift를 기록하며 특히 희소한 환경에서도 뛰어난 성능을 보여준다.
    

### ikd-Tree의 Incremental Update란 무엇인가, 이를 통해 Realtime Mapping에서 어떤 문제를 해결할 수 있는가

**Incremental Update**

기존의 k-d Tree는 새로운 Data를 삽입할 때 전체 Tree를 재구성해야 하는 한계가 있었다. 

Incremental Update는 삽입, 삭제 시 필요한 부분만 갱신하고 균형이 깨진 Sub Tree만 Re-Balancing하여 Realtime으로 Tree를 유지한다. 

**Realtime Mapping으로 해결할 수 있는 문제**

1. 지연 최소화
    
    새로운 LiDAR Data가 Realtime으로 입력될 때, 전체 Tree를 재구성하는 대신 필요한 부분만 수정해 작업 지연을 줄인다. 
    
2. Memory 효율성
    
    동적으로 Point를 삽입, 삭제하며 불필요한 Data를 제거해 Memory 사용량을 줄였다. 
    
3. 속도 향상
    
    kNN 검색과 Map Update가 보다 효율적으로 수행되어 실시간 Mapping 성능이 개선되었다. 
    

### FAST LIO2에서 Direct Point Registration 방식을 채택한 이유와 해당 방식이 다양한 LiDAR Sensor에 제공하는 적응성은 무엇인가

**Direct Point Registration 방식 채택 이유**

기존 LOAM 기반 Feature Extraction 방식은 특정 Feature를 추출하는데 계산 지원이 많이 필요했다. 

Direct Point Registration은 LiDAR Point Cloud를 바로 Map에 등록하므로 계산 부하를 줄이고 Data 전체 구조를 활용할 수 있다. 

**다양한 LiDAR Sensor에 대한 적응성**

Feature Extraction Module을 제거함으로서 LiDAR Scan Pattern과 관계 없이 Data를 처리할 수 있다. 

다양한 환경에서도 일관된 성능을 제공한다. 

### FAST LIO2가 Aggressive Motion이나 Sparse Environments에서도 높은 정확도를 유지하기 위해 사용하는 설계적 요소와 기술적 접근 방법

**설계적 요소** 

1. **Iterated Kalman Filter**
    
    반복적인 Filtering을 통해 Post Estimation 정확도를 높이고 빠른 움직임에서도 안정적으로 Data를 처리한다. 
    
2. **ikd-Tree 기반 Mapping**
    
    Point Cloud를 효율적으로 정렬 및 검색하여 희소한 Data 환경에서도 높은 정확도를 유지한다. 
    

**기술적 접근**

Data의 적응적 처리를 통해 빠른 회전 속도에서도 Pose Estimation이 가능하다. 

희소한 환경에서도 Registration 과정에서 Point Data를 최대한 활용해 Drift를 줄였다. 

### FAST LIO2의 처리속도가 초당 10-100Hz에 도달할 수 있었던 주요 요인과 다른 최신 기술에 비해 최대 10배 빠른 이유

**처리 속도 향상의 주요 요인**

1. **효율적인 Data 구조**
    
    ikd-Tree의 Incremental Update를 통해 Data를 Realtime으로 삽입, 삭제, 검색하여 지연 시간을 최소화한다. 
    
2. **Direct Point Registration**
    
    Feature Extraction을 제거해 계산 부하를 대폭 줄였다. 
    
3. **효율적인 Filter 설계** 
    
    Iterated Kalman Filter와 Efficient Kalman Gain을 활용해 Pose Estimation을 최적화했다. 
    

**다른 최신 기술과의 차이** 

기존 기술은 Tree 구조가 비효율적이거나 Point Cloud Data 처리에 많은 계산 부하가 필요했다. 

FAST LIO2는 이러한 한계를 극복하여 Realtime 처리 속도를 획기적으로 높였다.