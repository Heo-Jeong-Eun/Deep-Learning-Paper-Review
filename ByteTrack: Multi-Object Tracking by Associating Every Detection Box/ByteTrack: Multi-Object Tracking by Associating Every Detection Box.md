## Abstract

**Multiple Object Tracking**는 Video에서 **Object의 BBox와 ID를 추정하는 작업**이다. 

대부분의 방법은 특정 임계값보다 높은 신뢰도를 가진 BBox를 연관지어 Object의 ID를 할당한다. 이때 신뢰도가 낮은 BBox(가려진 Object)를 단순히 버리게 되면 실제 Object가 누락되고 경로가 단절되는 문제가 발생한다. 

이를 해결하기 위해 기존 방식과 달리 거의 모든 BBox를 연관짓는 새로운 Tracking 방식을 제안한다. 

**낮은 신뢰도의 BBox에 대해 Tracklets과의 유사성을 활용해 실제 Object를 복원하고 Background Detection을 Filtering 한다.** 

해당 논문에서는 9개의 최신 Tracker에 적용한 결과, IDF1 점수가 1-10 Point 향상되었다. 

또한 ByteTrack이라는 단순하면서도 강력한 Tracker를 설계해 MOT17 Testset에서 80.3 MOTA, 77.3 IDF1, 63.1 HOTA를 30FPS 속도로 달성했다. 

ByteTrack은 MOT20, HiEve, BDD100K등의 Dataset에서도 최첨단 성능을 기록했으며 Source Code 및 Pre-Trained Model이 공개되었다. 

---

기존 Multiple Object Tracking 방법은 신뢰도가 늦은 BBox을 버리지만 ByteTrack은 거의 모든 BBox를 고려해 Object를 보다 정확하게 Tracking한다. 

이를 통해 MOTA, IDF1등의 성능이 향상되었고 MOT17, MOT20등 다양한 Benchmark에서 최고 성능을 달성했다. 

## Conclusion

해당 논문에서는 Multiple Object Tracking을 위해 간단하면서도 효과적인 Data 연관 방법인 BYTE를 제안했다. 

BYTE는 기존 Tracker에 쉽게 적용이 가능하며 일관된 성능 향상을 제공한다. 

또한 강력한 Tracker인 ByteTrack를 개발해 MOT17 Testset에서 80.3 MOTA, 77.3 IDF1, 63.1 HOTA를 30FPS로 달성하며 1위를 기록했다. 

ByteTrack은 Occlusion에 강하고 정확한 Detection을 수행하며 낮은 신뢰도의 BBox를 활용한 연관 과정을 통해 ID Switch가 최소화된다. 

---

**ByteTrack은 단순하면서도 강력한 Multiple Object Tracking으로 낮은 신뢰도의 BBox를 활용해 ID Tracking의 연속성을 높이고 성능을 극대화**한다. 

다양한 Benchmark에서 최고 성능을 달성하며 실제 App에서도 활용 가능성이 높다. 

## Results & Discussion

### Key Experimental Results

- 80.3 MOTA, 77.3 IDF1, 63.1 HOTA, 30FPS
- 기존 최고 성능 대비 **+3.3 MOTA, +5.3 IDF1, +3.4 HOTA** 향상

### MOT20 Benchmark 1st

- 77.8 MOTA, 75.2 IDF1, 61.3 HOTA
- 기존 최고 성능 대비 **ID Switch 71% 감소**

### BDD100K(Autonomous Driving Dataset) Performance

40.1 mMOTA, 55.8 mIDF1, 69.6 MOTA → **모든 기존 Model보다 우수**

### HiEve(Complex Event-Inclusive Dataset) Performance

61.7 MOTA, 63.1 IDF1, 38.3% MT → 기존 최고 Model 대비 성능 향상

### Results of Applying the BYTE Method to 9 Existing SOTA Trackers(BYTE 방법을 기존 9개의 SOTA Tracker에 적용한 결과)

- **모든 Model에서 MOTA, IDF1 개선**
- CenterTrack : **IDF1 +9.8 향상**
- Chained-Tracker : **MOTA +1.9, IDF1 +5.8 향상**

## Introduction

### Limitations of Existing Methods

- MOT는 일반적으로 Tracking-by-Detection 접근 방식을 사용한다.
- 대부분의 방법이 BBox의 Score(신뢰도) Threshold(임계값)을 설정해 낮은 Score의 BBox를 제거한다.
- 낮은 신뢰도 BBox는 실제 Object(가려진)를 포함할 수 있으며 이를 제거하면 Object 누락과 ID 단절이 발생한다.

### Key Ideas of ByteTrack

- 모든 BBox를 활용해 낮은 신뢰도의 BBox를 기존 Tracklet과 연관시킨다.
- **Motion Model + IoU 기반의 2단계 Matching(연관)**기법을 적용
    
    **ByteTrack은 Detection 된 Object를 Tracking 하는 과정에서 두 단계로 Matching 작업을 수행한다.** 
    
    1. **High Confidence Matching** → 기존 방법은 1단계까지 진행 
        
        **신뢰도 높은 BBox만 Tracklet과 Matching**
        
        - 먼저 Confidence Score(Detection 점수)가 높은 BBox만 이용해 기존 Track(Tracklet)과 Matching 한다.
        - **Matching Method**
            - Motion Model, Kalman Filter를 사용해 이전 Frame의 Object가 어디있을지 Prediction
            - Prediction 된 위치와 Detection 된 BBox 사이의 IoU(Intersection over Union)을 계산해 가장 잘 맞는 것과 Matching 한다.
        - 이 단계에서 Matching 되지 않은 Object(신뢰도가 낮은 BBox로 인해 누락된 Tracklet)가 남게 된다.
    2. **Low Confidence Matching** → 하지만 ByteTrack은 2단계 Matching을 수행
        
        **남은 Tracklet을 신뢰도 낮은 BBox와 추가 Matching**
        
        - 1차 연관에서 Matching 되지 않은 Tracklet과 BBox 중 신뢰도가 낮은 BBox를 사용해 다시 한번 Matching 한다.
        - 이때 Re-ID(Appearance Feature Matching) 같은 복잡한 기법을 사용하지 않고 단순히 IoU 기반으로 Matching 한다.
        - 이 과정에서 기존에는 버려졌을 낮은 신뢰도 BBox들도 유용하게 활용되어 Object를 Tracking 하는데 기여할 수 있다.
- 이 과정에서 Background Noise는 걸러지면서 신뢰도 낮은 Object들은 유지된다.

### Key Contributions

1. BBox의 신뢰도에 의존하지 않는 Tracking 기법 제안
2. 기존 9개의 SOTA Tracker에 적용해 성능 향상 증명
3. 새로운 MOT Tracker인 ByteTrack 개발 → Benchmark 1위 기록
4. 실제 응용 가능성 높은 속도, 30FPS와 성능을 동시에 달성 

## Model Structure

### Object Detection

ByteTrack은 Object Detection을 위해 YOLOX를 사용한다. 
Detector의 역할은 각 Frame에서 BBox를 예측하고 Confidence Score를 부여한다. 

```
Input : Video Frame
Output : BBox Coordinates + Confidence Score
```

### Kalman Filter for Motion Prediction

1. 현재 Frame의 Tracklet 위치를 Update → Prediction
2. 다음 Frame에서 Detection 된 BBox와 비교해 Object를 Matching → Association
3. Prediction 값과 실제 Detection 된 BBox를 보정해 더 정확한 위치를 추정한다. 

### Two-Stage Data Association

ByteTrack의 Key Idea는 **Confidence Score(Detection 신뢰도)에 따라 두 단계로 Matching 하는 것**이다. 

1. High Confidence Matching
    - 먼저 신뢰도 높은 BBox만 기존 Tracklet과 Matching
    - IoU or Re-ID 기반 유사도를 계산해 가장 적합한 Tracklet과 연결한다.
2. Low Confidence Matching
    - High Confidence Matching에서 Matching 되지 않은 Tracklet과 신뢰도 낮은 Detection BBox를 추가적으로 Association 한다.
    - 해당 과정에서 Background Noise는 걸러지고 Occluded Object들은 다시 복원될 가능성이 높아진다.
    - 단순한 IoU Matching을 사용해 가려진 Object라도 ID를 유지할 수 있도록 한다.

Two-Stage Data Association으로 기존 MOT 기법에서 발생하던 Object ID Switch 문제를 크게 줄일 수 있다.  

### Track Management

Detection 된 Object들이 Matching 되지 않은 경우를 대비해 ByteTrack은 Tracklet Management 기법을 사용한다. 

- **Tracklet Maintenance Policy**
    - Matching 되지 않은 Tracklet을 즉시 삭제하지 않고 일정 Frame 동안 유지한다. (약 30 Frame정도 유지)
    - 다시 Detection 되면 이전 ID를 유지한 채 Tracking을 이어간다. → ID 유지 성능 증가
- **Create a New Object**
    - Detection 된 BBox 중 이전 Tracklet과 Matching 되지 않은 BBox들은 새로운 Object로 간주해 새로운 Tracklet을 생성한다.
- **Remove Background Detection**
    - 낮은 신뢰도를 가진 Detection Box가 배경일 가능성이 높다.
    - Two-Stage Data Association 이후에도 Matching 되지 않은 BBox는 배경으로 판단하고 제거한다.