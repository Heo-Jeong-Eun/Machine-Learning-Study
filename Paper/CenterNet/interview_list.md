# CenterNet Interview List
    
### 1. CenterNet이란?    
CenterNet은 객체 검출 모델로 객체가 있을법한 잠재적 위치를 추정해서 검출하는 일반적인 모델과 다르게    
key point를 찾아 단일 지점으로 객체를 검출하는 모델입니다.    
Bbox를 추정하는 모델들보다 간단하며 빠른 성능을 보유하고 있습니다.    
    
### 2. NMS란 무엇인가요?
<img src="https://wikidocs.net/images/page/142645/NMS.png"
    width="50%"
    height="50%"/>
<!-- ![img](https://wikidocs.net/images/page/142645/NMS.png) -->
NMS(Non-Maximum Suppression)란 객체가 존재하는 위치 주변에 여러개의 Bbox가 생성되는데 그 중 score가 높은 1개의 bbox로 추출하는 알고리즘입니다.    
기존의 객체검출에서 사용하는 NMS는 IoU(Intersection over Union)의 값을 정하여 배경과 객체로 검출하거나 bbox를 무시하게 됩니다.    
예를 들어, IoU값이 0.7이상이라면 객체로 인식하고, IoU값이 0.3이하라면 배경으로 인식하거나 무시하게 됩니다.    
#### IoU란
<img
    src="https://blog.kakaocdn.net/dn/1LCYM/btq93o4JkR4/IlOWv5Z5uKtlBSXBQJHTck/img.png"
    width="50%"
    height="70%"/>
<!-- ![img](https://blog.kakaocdn.net/dn/1LCYM/btq93o4JkR4/IlOWv5Z5uKtlBSXBQJHTck/img.png) -->
2개의 영역이 얼마나 겹쳐있는지를 확인하는 수치로 겹친영역/전체영역 으로 나타낸다.    
    
### 3. Key Point를 사용한 검출이 CenterNet이 최초는 아니지 않나?
네, 맞습니다.    
CenterNet 이전 CornerNet과 ExtremeNet에서도 Key Point를 사용한 객체를 검출하였습니다.    
하지만 가장 큰 차이점은 연산량입니다.    
2개의 Key Point를 사용하는 CornerNet, 5개의 Key Point를 사용하는 ExtremeNet에 비해    
CenterNet은 단 1개의 Center Point를 사용하여 객체를 검출하기 때문에 이전의 Key Point 검출 방법에 비해 속도가 빠릅니다.    
    
### 4. One Stage를 사용하여 객체를 검출하는 CenterNet
객체를 검출할 때 labeling과 classificaion을 동시에 하는 One Stage 방식과    
classification, labeling을 나누어 단계적으로 검출하는 Two Stage 방식이 있습니다.    
One Stage방식은 한번에 검출하기 때문에 속도는 좋지만 정확성은 Two Stage에 비해 다소 떨어지는 성향이 있으며,    
Two Stage방식은 One Stage에 비해 속도는 느리지만 정확성은 훨씬 좋은 성향을 가지고 있습니다.    
    
### 5. L1 loss를 사용하는 CenterNet
Loss에는 실제값에서 예측값을 뺀 후 절대값을 다 더하여 loss를 구하는 L1 loss와    
실제값에서 예측값을 뺀 후 절대값에 제곱을 취한 후 더하는 L2 loss가 있습니다.    
CenterNet에서 사용하는 loss는 L1으로 이상치에 대해 민감하지 않는 loss를 사용하고 있습니다.    
(만약 이상치에 대해 주의 깊게 확인하고 싶은 데이터일 경우, L2 loss를 사용해야한다.)
