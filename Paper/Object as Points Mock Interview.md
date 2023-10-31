# Object as Points Mock Interview
Author **Heo-Jeong-Eun**

### CenterNet이란 ?

CenterNet이란 Anchor Box를 사용하는 기존의 **One-Stage Detector**와 비슷한 접근 방식을 보이는 Objcet Detection 모델이다. 

다만, CenterNet은 미리 정해진 Anchor에서 BBox Overlap을 기준으로 삼는 것과 달리, **각 Object에 대해 단 하나의 중심점을 Key Point Estimation 방식으로 추정**한다. (Anchor Free Model)

(기존 One-Stage Detector들은 각 Anchor 위치에서 정해진 비율의 BBox들에 대해 Overlap 정도를 기준으로 학습하지만 CenterNet은 **Feature Map으로부터 각 Object의 중심점 위치를 확률 Map으로 나타내어 학습**한다. 즉 **중심점 위치를 Key Point로 정의**하고 문제를 해결한 것이다.)

또한 CenterNet은 더 큰 Output Resolution(Output Stride of 4)을 가진다. 

### Output Stride란 ?

Output Stride는 **CNN 구조에서 입력 이미지와 Output Feature Map 사이의 크기 비율을 나타내는 개념**이다. 
보통 CNN은 여러 층의 Convolution Layer와 Polling Layer로 구성되어 있다. 
입력 이미지가 이 네트워크를 통과하면서 크기가 줄어드는데, 이때 Output Stride는 입력 이미지의 크기와 Output Feature Map 크기 간의 비율을 의미한다. 

작은 Output Stride 값은 공간 해상도를 높이지만 더 많은 계산을 필요로 하며, 큰 Output Stride 값은 계산을 줄이지만 공간 해상도를 낮출 수 있다. 

**Output Resolution**은 **Object를 감지하기 위한 최종 Map**의 크기로, Output Stride of 4인 경우, **입력 이미지가 네트워크를 통과한 후 Output Feature Map의 크기가 입력 이미지의 크기보다 4배 작아진다는 것을 의미**한다.

즉, 네트워크는 입력 이미지를 처리하여 최종 Output Feature Map을 생성하며 이 Map은 입력 이미지보다 4배 작은 공간 해상도를 갖게 된다. 이를 통해 네트워크는 Object의 중심점을 예측하고 해당 Center를 기반으로 BBox를 생성할 수 있다. 

### Output Feature Map이란 ?

**CNN의 중간 단계에서 얻어지는 3차원 배열**이다. 이는 입력 이미지를 Convolution Layer와 Pooling Layer를 통과시켜 얻어지는 중간 결과물로서 각 Channel은 특정한 특징을 나타낸다. 

1. **Spatial Dimension** (공간 차원) : Output Feature Map은 공간적인 정보를 포함한다. 이는 가로, 세로 차원으로 표현된다. 
2. **Channel** (채널) : Output Feature Map은 여러 개의 Channel로 이루어져있다. 각 Channel은 특정한 시각적, 의미적인 특징을 갖는다. 
3. **Depth** (깊이) : Output Feature Map의 Depth는 해당 Layer에서 사용된 Filter의 갯수에 의해 결정된다. 각 Filter는 입력 데이터의 특징을 감지하고 나타낸다. 

### One-Stage Detector와 Two-Stage Detector

**One-Stage Detector**

**One-Stage Detector란 한 단계에서 Object의 존재와 위치를 예측**한다. 즉, **Object의 BBox와 Class를 한번에 예측**한다. 

**Grid 기반의 예측**을 통해 격자 **Cell마다 여러 Class의 확률과 BBox 정보를 생성**하는 구조를 가지고 있다. 

주로 **다양한 크기와 종횡비를 가진 Anchor Box**를 사용하여 예측을 수행한다. 

One-Stage Detector는 일반적으로 Two-Stage Detector에 비해 **빠른 속도**로 Object Detection을 수행한다. 따라서 **실시간 처리가 필요한 상황에 유용**하다. 

One-Stage Detector는 **Hard Negative Mining 과정이 필요하지 않다.** (Hard Negative Mining : 학습 중 어려운 Sample을 처리하는 과정)

일반적으로 One-Stage Detector는 **모델의 구조가 간단**하고 **파라미터 수가 적다.** 

작은 Object나 밀집된 Object들에 대해서도 비교적 잘 동작한다. 

**Two-Stage Detector**

**Two-Stage Detector는 두 단계로 Object 검출을 수행**한다. 첫번째 단계에서는 **후보 영역(Region Proposal)을 생성**하고, 두 번째 단계에서는 **각 후보 영역을 분류하고 BBox를 조정**한다. 

첫 번째 단계에서 **Region Proposal 네트워크를 사용해 후보 영역을 생성**한다. 

Two-Stage Detector는 **각 단계가 복잡하고 연산량이 많기 때문에** 처리 시간이 더 오래 걸릴 수 있다. 

일반적으로 Object의 크기나 종횡비에 민감하기 때문에 작은 Object, 비균일한 Object의 크기를 감지하기 어렵다. 

두 단계로 나뉜 구조 때문에 Hard Negative Mining 과정이 필요할 수 있다. 

Two-Stage Detector는 더 복잡한 모델 구조를 사용하고, 두 단계로 나누어 Obejct를 검출하기 때문에 정확한 검출이 가능하다. 

**One-Stage Detector VS Two-Stage Detector**

One-Stage Detector가 더 빠르기 때문에 속도가 중요한 실시간 처리가 필요한 경우 적합하다. 

Two-Stage Detector는 더 복잡한 모델 구조로 정확도가 중요한 경우 적합하다. 

다양한 크기와 종횡비의 Object를 처리해야하는 경우 One-Stage Detector가 더 유리하다. 

복잡한 Object 상황을 다루어야하는 경우 Two-Stage Detector가 더 유리하다. 

### Key Point Estimation이란 ?

**CenterNet은 Key Point Estimation을 활용해 Obejct의 중심점을 찾는다. Key Point Estimation 방식을 사용하면 Object의 중심을 정확하게 찾을 수 있기 때문에 작은 Object와 겹친 Object에서 좋은 성능을 보인다.** 

방법은 다음과 같다. 

1. 중심점 예측 : CenterNet은 이미지 내에서 Object의 중심은 Key Point로 Object의 중앙에 해당한다. 
2. 크기 예측 : Object의 크기를 예측해 BBox를 생성한다. 
3. Key Point Estimation 활용 : 예측된 중심점을 사용해 Object의 위치를 정확하게 파악하고 BBox를 생성한다. 

**Key Point Estimation은 이미지 내에서 특정 Object나 부분 Key Point(Landmark)를 찾는 작업**이다. Key Point는 Object의 중요한 부분을 나타낸다. 

주요 단계는 다음과 같다. 

1. 데이터 준비 : Key Point 추정 작업을 위해서는 훈련 데이터셋이 필요하다. 이 데이터셋에는 이미지와 해당 이미지에 대한 정확한 Key Point 위치 정보가 포함되어 있어야 한다. 
2. 모델 구조 : Convolutional Neural Network 기반의 네트워크를 사용하여 Key Point를 예측한다. 이때 네트워크는 이미지를 입력으로 받아 Key Point의 위치를 출력으로 생성한다. 
3. 손실 함수 : 예측된 Key Point와 실제 Key Point 사이의 오차를 계산하는 손실 함수를 정의한다. 
4. 훈련 : 데이터셋을 사용해 모델 훈련을 시킨다. 이 과정에서 모델은 이미지를 입력으로 받아 Key Point를 예측하고, 손실 함수를 최소화하도록 업데이트된다. 

### BBox 형태의 Detection이 아닌 Key Point Detection을 사용하는 것이 성능이 좋은 이유는 ?

비효율적인 Post-Processing 과정이 필요 없기 때문이다. 

### Post-Processing이란 ?

**초기 모델 출력을 개선하거나 추가적인 정보를 얻기 위해 모델의 출력에 대해 추가적인 단계나 변환을 적용하는 과정**을 의미한다. 

Object Detection에서 Post-Processing : 모델의 출력은 종종 BBox의 좌표, Class의 확률 등을 포함하는데 이 정보를 기반으로 최종적인 BBox를 생성하거나 불필요한 Box를 제거하는 과정이 있다. 

### CenterNet 구조

<img src = 'image/CenterNet Structure.png'>

ncls = number of class / ncls x 64 x 123은 출력 tensor의 모양이다. 

**CenterNet은 Feature를 생성하는 Backbone과 3개의 Header**로 이루어져있다. Header는 Heatmap, Offset, Width & Height를 출력한다. 

**Center Heatmap**은 Channel Index가 각 Object Category에 해당한다. 
예를 들어 사람의 Object ID = 0일 때 CenterHeatmap[0, :, :]에는 사람의 Center 위치에 해당하는 Pixel의 값이 1에 가까울 것이다. 

**Center Offset**은 Output Stride 때문에 발생하는 Discretization Error를 보상하기 위해 추정하는 것으로 각 Object Center 위치를 입력 이미지 Scale로 해석할 때 오차를 보상하기 위한 값이다. 
예를 들어 Input 이미지에서 A라는 Object Center가 (146, 133)에 위치한다면 출력 Center Heatmap Tensor에서 해당 Object 위치는 (36, 33)이다. 하지만 실제 (146 / 4, 133 / 4) = (36.5, 33.25)이므로 (0.5, 0.25)의 오차가 발생한다. 이 오차가 Output Stride에 의해 발생하는 Discretization Error이고, 이를 보상하기 위해 추정한다. 

**Width & Height** Tensor는 Object의 Width, Height를 추정하는 Tensor이다. 
예를 들어 Object ID = 0인 물체의 Width, Height = WidthAndHeight[0 : 2, x, y] 위치 값이고 Object ID = 4인 물체의 Width, Height = WidthAndHeight[8 : 10, x, y]에 저장된다.  

### Offset이란 ?

Objcet의 **중심점으로부터 상대적인 위치를 나타내는 값**으로 Obejct를 감지하기 위해 사용되며, Objcet의 Center를 기준으로 한 상대적인 위치를 나타내기 때문에 일반적으로 양수 또는 음의 실수로 표현된다. 

예를들어 Object의 Center가 이미지 중앙에 위치하면 Offset 값은 (0, 0)이 될 것이다. 만약 Object가 중앙에서 오른쪽으로 10 Pixel, 위쪽으로 5 Pixel 떨어져있다면 Offset 값은 (10, -5)가 될 것이다. 

### Tensor란?

PyTorch의 Tensor는 **다차원 배열**로, 수치 연산을 위한 핵심 데이터 구조이다. Tensor는 다차원 Tensor의 크기, 형상, 데이터 유형 등을 표현한다. 
PyTorch에서 Tensor는 GPU 상에서도 연산이 가능하며, 자동 미분을 지원하여 딥러닝 모델의 학습에 유용하게 사용된다.

Tensor는 다양한 수학적 연산을 제공하며, 행렬 연산, 선형 대수, 통계 분석 등 다양한 분야에서 활용된다. 또한, Tensor는 신경망 모델의 입력 데이터, 가중치, 편향 등을 표현하고 처리하는 데에도 사용된다.

### ConnerNet과 CenterNet의 차이점 ?

CenterNet은 Object의 중심점을 찾는 방식으로 동작한다. 이 모델은 Object의 중심점과 크기를 예측하고 이를 이용해 BBox를 생성한다.
ConnerNet은 BBox의 꼭짓점을 예측하여 Object를 표현한다. 

CenterNet은 주로 Feature Map의 각 Pixel에 대한 분류, 회귀 및 크기 예측을 수행하기 위해 Hourglass 구조를 사용한다. 
ConnerNet은 네트워크를 통해 Object의 꼭짓점을 예측하고 이를 이용해 BBox를 생성한다. 

ConterNet은 기존의 Object 검출 방법보다 정확하게 중심을 찾을 수 있어 작은 Object나 겹쳐진 Object를 잘 처리할 수 있다. 하지만 큰 Object나 밀집된 Object에 대한 처리는 상대적으로 어려울 수 있다. 
ConnerNet은 Object의 꼭짓점을 예측하기 때문에 Object의 형태를 더 정확하게 표현할 수 있다. 이는 Object의 회전과 같은 변형을 잘 처리할 수 있도록 해준다. 하지만 불필요한 꼭짓점이 예측될 수 있어 후처리 과정이 필요하다.  

### Hourglass란 ?

Hourglass 구조는 **Object Detection 및 인식 작업을 수행하는 딥러닝 네트워크 구조** 중 하나이다. 
다단계로 구성된 네트워크로 각 단계에서는 입력 이미지의 해상도가 감소하고, 이 과정에서 Object의 정보를 추출한다. 

Hourglass는 **Incoder**와 **Decoder**로 나뉜다. 
**Incoder**는 입력 이미지에서 특성을 추출하는 역할을 한다. 일반적으로 Convolution Layer를 사용하여 이미지의 다양한 특징을 추출한다. 
**Decoder**는 Incoder에서 얻은 특성을 이용하여 원래의 해상도로 복원하고, Object의 중심점과 크기를 예측한다. Decoder Up-Sampling Layer를 사용하여 해상도를 높이는 과정을 포함한다. 

**Hourglass 구조에서는 각 단계의 Incoder와 Decoder 사이 Skip 연결이 존재한다.** 이를 통해 고해상도 정보와 저해상도 정보를 결합하여 정확한 예측을 가능하게 한다. 

Hourglass는 Incoder와 Decoder를 여러 번 반복하여 사용할 수 있다. 이를 통해 더 깊은 네트워크를 만들고 더 복잡한 특징을 추출할 수 있다.