## Abstract

Simple Online and Realtime Tracking은 단순하고 효과적인 Algorithm을 중심으로 한 실용적인 Multiple Object Tracking 방식이다. 

본 논문에서는 SORT의 성능을 향상시키기 위해 Deep Learning 기반 Appearance Information(외형 정보)를 추가했다.

이러한 확장을 통해 Occlusion(차단) 기간 동안 Object를 Tracking할 수 있으며 ID Switch 횟수를 효과적으로 줄일 수 있다. 

### Issue

기존 SORT는 단순하고 빠르지만 Occlusion이 발생할 경우 ID Switch가 빈번하게 발생한다. 

### Approach

Deep Learning 기반의 Appearance Information(Features)를 추가해 SORT의 Deep Association(심층 연관) Metric을 개선한다. 

### Method

대규모 Person Re-Identification(보행자 재식별) Dataset을 활용해 Deep Association(심층 연관) Metric을 Pre-Training

Online에서는 Nearest Neighbor(최근접 이웃) 검색을 활용해 Object와 Tracking 경로를 연결한다. 

### Results

ID Switch 횟수 45% 감소

Realtime Preformance 유지 

경쟁력 있는 Multiple Object Tracking 성능 달성 

## Conclusion

Pre-Trained Association Metric을 통해 Appearance Information을 통합한 SORT 확장 방법을 제안한다. 

더 긴 Occlusion 기간동안 Object를 효과적으로 Tracking할 수 있으며 이를 통해 SORT는 최신 State of the Art Online Tracking Algorithm과 경쟁할 수 있는 강력한 기법이다. 

해당 Algorithm은 여전히 구현이 간단하며 Realtime 적용이 가능하다. 

### Propose

기존 SORT에 Appearance Feature를 추가, Association Metric을 개선한다. 

### Key Improvement

긴 Occlusion 상황에서도 Tracking이 가능하다. 

최신 Online Tracking 기법과 경쟁이 가능하다. 

### Advantage

Simple to Implement(구현)

Real Time Proformance

## Results & Discussion

| 성능 지표 | 기존 SORT | Deep SORT | 개선 효과 |
| --- | --- | --- | --- |
| **ID 전환 수** | 1423 | 781 | **45% 감소** (ID 유지 성능 향상) |
| **Mostly Tracked(MT)** | 25.4% | 32.8% | **Tracking 성공률 증가** |
| **Mostly Lost (ML)** | 22.7% | 18.2% | **Tracking 실패율 감소** |
| **FPS(실행 속도)** | 60 FPS | 40 FPS | **실시간 성능 유지** |
| **MOTA(Tracking 정확도)** | 비교적 높은 정확도 | 유사하게 높은 정확도 | **ID 전환을 줄이면서도 높은 성능 유지** |

본 논문의 실험 결과는 MOT16 Benchmark를 사용해 평가 되었다. 

MOT16 Benchmark는 Multiple Object Tracking 성능을 평가하는 표준 Dataset으로 다양한 Tracking 성능 지표(Metrics)를 활용해 Algorithm의 성능을 비교할 수 있다. 

본 연구에서는 Deep SORT와 기존 SORT를 비교했다. 

### Identity Switch

Identity Switch가 약 45% 감소되었다. 

| 기존 SORT의 ID Switch 횟수 | Deep SORT의 ID Switch 횟수 |
| --- | --- |
| 1432번 | 781번 |

기존 SORT는 오직 위치 기반 정보만을 사용해 Object를 Tracking했다. 

Deep SORT는 여기에 Appearance Feature를 추가해 같은 Object의 시각적 특성을 고려, 더욱 정확하게 Tracking이 가능해졌다. 

즉, Object가 일시적으로 가려졌다가 나타나도 이전 외형 특징을 기억하고 있어 동일한 ID를 유지할 가능성이 높아진 것이다. 

### Improved Tracking Performance

Tracking 성능을 평가하는 주요 지표인 Mostly Tracked 비율과 Mostly Lost 비율이 향상되었다. 

| Mostly Tracked | Mostly Lost |
| --- | --- |
| 기존 SORT에 비해 MT 비율이 증가했다.  | 기존 SORT에 비해 ML 비율이 감소했다.  |
| 25.4% → 32.8%  | 22.7% → 18.2% |
| 특정 Object가 오랜 시간동안 안정적으로 Tracking 되었다면 MT 값이 높아진다.  | Object가 등장했지만 거의 Tracking 되지 못한 경우 ML 값이 높아진다.  |

즉, Deep SORT는 기존 SORT보다 더 오랜 기간 동안 Obejct를 안정적으로 추적할 수 있다. 

### Maintain Realtime Performance

Deep SORT에서 실행 속도는 40FPS이다. 이는 Realtime에서 동작할 수 있는 수준이다.  

ID Switch, Tracking 성능 향상이 있었음에도 연산 속도가 크게 저하되지 않았음을 의미한다. 

## Introduction

Online Tracking 환경에서는 각 Frame에서 즉시 Object의 ID를 결정해야 하는 경우가 많기 때문에 Flow(흐름) Network, Probabilistic(확률) Graphical Model 등의 접근 방식으로 Batch를 처리하는 것은 적절하지 않다. 

즉, Online Tracking에서는 Global Optimization(전역 최적화) 방식이 아닌 Frame 단위 처리 방식이 필요하다. 

SORT는 Kalman Filter와 Hungarian Algorithm을 활용한 Frame 단위 처리 방식을 사용한다. 

따라서 SORT는 Online Tracking이 가능하며 Global Optimization 보다 훨씬 단순하고 계산량이 적어 Realtime 적용이 가능하다. 

하지만 ID Switch가 많고 Occlusion 상황에서 성능이 떨어지는 단점이 있기 때문에 Deep SORT에서는 이를 개선하기 위해 CNN을 활용한 Appearance Features를 추가해 Tracking 성능을 높인다. 

Object의 Motion Info 뿐 아니라 Appearance Info도 결합해 Tracking 성능을 향상시키는 것이다. 

이를 위해 대규모 Person Re-Identification(보행자 재식별) Dataset에서 학습된 CNN을 활용했다. 

이 CNN을 통합함으로서 Misses(누락), Occlusion(차단) 상황에서도 더 강인한 Tracking이 가능하다. 
또한 Online Tracking 환경에서도 구현이 간단하고 효율적이며 Realtime 적용이 가능하다.