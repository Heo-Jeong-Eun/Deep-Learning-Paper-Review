## Q&A

### 기존 SORT와 Deep SORT의 가장 큰 차이점은 무엇인가

기존 SORT는 Object의 위치 정보(BBox)만을 이용해 Tracking을 수행한다. 

Deep SORT는 Appearance Feature를 추가해 ID 유지 성능을 향상시킨다. 

### DeepSORT에서 CNN을 활용한 Appearance Descriptor는 어떻게 작동하는가

Object의 Appearance Feature를 학습하기 위해 CNN을 활용한다. 

1. 대규모 **Person Re-identification, Re-ID Dataset**을 사용하여 CNN을 Pre-training 한다. 
2. 학습된 CNN은 **각 Object의 Feature을 128차원의 Vector(Feature Embedding)로 변환**하여 저장한다. 
3. 이후, Frame 간 Object를 Matching할 때 **이전 Frame의 Appearance Feature와 현재 Frame의 Feature 간 유사도를 비교**하여 Data Association을 수행합니다.

### Deep SORT에서 Data Association을 위한 두 가지 주요 Metric은 무엇인가

1. **Motion** 기반 **Mahalanobis** Distance(마할라노비스 거리)
2. **Appearance** 기반 **Cosine** Distance

### 실험 결과에서 Deep SORT가 기존 SORT보다 우수한 점은 무엇인가

MOT16 Banchmark 실험 결과 Deep SORT는 기존 SORT 대비 **ID 유지 성능이 향상되었으며, 실시간 성능을 유지**했다. 

### Deep SORT의 한계점은 무엇인가

1. **Object Detction에 성능 의존**
    
    Deep SORT는 Object Detection이 실패하면 Tracking도 실패하는 구조로 Detector의 성능이 낮으면 Tracking 정확도도 낮아진다. 
    
2. **Camera Motion에 취약**
    
    Mahalanobis Distance는 고정된 환경에서 유용하지만 Camera가 움직이는 경우 신뢰도가 낮아질 수 있다. 
    
3. **Computational Cost**
    
    CNN을 활용한 Appearance Feature Extraction이 추가됨에 따라 기존 SORT보다 연산량이 증가한다. 
    
    SORT는 60 FPS로 동작했지만 Deep SORT는 40 FPS로 감소했다.