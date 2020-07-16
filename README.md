# Free-Form-Image-Inpainting-with-Gated-Convolution
#ICCV 2019 #GAN #Image Inpainting #Computer Vision

# Introduction
컴퓨터 비전에는 이미지 인 페인팅에 대한 두 가지 광범위한 접근 방식이 있다. 저수준 이미지 피쳐를 사용한 패치 매칭과 딥 컨벌루션을 사용한 피드 포워드 생성 모델이다. 전자의 접근 방식은 그럴듯한 정적인 텍스처를 합성 할 수 있지만 복잡한 장면, 얼굴 및 물체와 같은 정적이지 않은 경우에는 일반적으로 심각한 오류를 만든다. 후자의 접근 방식은 대량의 훈련 세트에서 학습 된 결과를 활용하여 정적이지 않은 이미지의 컨텐츠를 엔드 투 엔드 방식으로 합성할 수 있다. 
# Comparison of different approaches
![Comparison of different approaches](https://user-images.githubusercontent.com/59387983/87162966-a04ce900-c301-11ea-920a-d412c081c615.PNG)

Image Inpainting 분야에서 대표적인 Branches와의 비교에서 논문에서 제시한 모델은 데이터가 가진 정보를 의미적으로 학습하고, 글로벌한 영역에서의 데이터를 반영한다. 또한, 마스크의 형태를 자유롭게 설정할 수 있고, 사용자가 유도하는 데이터를 반영할 수 있다.

# Vanilla Convolution
![vanilla convolutions(1)](https://user-images.githubusercontent.com/59387983/87162684-3d5b5200-c301-11ea-9762-7df9f2a54d2b.PNG)
![vanilla convolutions(2)](https://user-images.githubusercontent.com/59387983/87162689-3df3e880-c301-11ea-9b0a-4ea007204f5e.PNG)

Vanilla Convolution은 몇가지 이유에서 Free-form image Inpainting에 적합하지 않다.

1. 모든 공간적 위치에서 동일한 필터를 적용하는데, 이는 모든 픽셀로부터 sliding-window를 통해서 지역적 특징을 추출해야하는 이미지 분류나 객체인식에 적합하다. 즉, 입력 데이터의 모든 픽셀이 유효하다는 것이다.

2. Image Inpainting에서 입력 데이터는 holes의 외부 유효한 픽셀/특징과 얕은 층에서 유효하지 않은 픽셀/특징 또는 깊은 층에서 마스크된 내부의 합성되는 픽셀/특징으로 구성된다. 이는 학습시 color discrepancy, blurriness and obvious edge responses와 같은 명료하지 않음을 유발한다.


# Partial Convolution
![partial convolution](https://user-images.githubusercontent.com/59387983/87162702-3fbdac00-c301-11ea-96ee-ed95abd7dd47.PNG)

Partial convolution은 정형화되지 않은 마스크에서 복원이미지의 품질을 향상시키지만, 여전히 몇몇 이슈가 있다.

1. 공간적인 위치를 휴리스틱하게 유효한지에 대해서 구별한다. 이는 다음 레이어의 마스크는 이전 레이어의 필터 범위에 포함되는 픽셀 수에 관계없이 마스크로 설정된다는 것을 말한다. 예를 들어, 현재 마스크를 업데이트하기 위해 1 개의 유효한 픽셀과 9 개의 유효한 픽셀이 동일하게 처리되는 것을 말한다.
2. 사용자의 추가적인 입력 데이터를 처리하는데 적합하지 않다.
3. 유효하지 않은 픽셀이 깊은 층에서 점진적으로 사라지며, 서서히 모든 마스크가 하나의 값으로 합성된다.
4. 모든 채널은 유연성이 제한된 상태로 동일한 마스크를 공유하며, 본질적으로 학습 불가능한 단일 채널의 특징 하드 게이팅으로 볼 수 있다.


# Gated Convolution
![functions of gated convolution](https://user-images.githubusercontent.com/59387983/87162693-3e8c7f00-c301-11ea-8168-56d6bae52cc2.PNG)

수식과 개념은 쉽다. 단지 이런 걸 착안할 수 있는 배경지식과 환경이 문제다. 일어나서 서 하겠습니다~

![Ilustration of convolutions](https://user-images.githubusercontent.com/59387983/87162695-3f251580-c301-11ea-83bb-f31f2e0759e4.PNG)


# Architecture
![Overview of SN-PatchGAN](https://user-images.githubusercontent.com/59387983/87162699-3f251580-c301-11ea-9975-395880be246a.PNG)

# Contextual Attention

Contextual Attention에 대해 논문에서 여러 장점을 언급정도로 소개하고 있어서 이에 대해 알아볼 필요가 있다. 이에 대해 구체적으로 발표한 논문이 Generative Image Inpainting with Contextual Attention이고, 아래에서 Contextual Attention에 대해 알아보겠다. 알아보니 이 논문의 저자가 마찬가지로 Contextual Attention을 고안했다. 지금은 구글 브레인에서 근무하시는 듯하다. 역시 좋은 논문을 쓴 저자는 좋은 회사에 들어가나보다. 아 모르겠다 진짜.. 아 물어볼 사람도 없고 답답하다...

contextual attention layer에서 알려진 백그라운드 패치로부터 피처의 정보를 가져와서 누락된 패치를 생성할 위치를 배운다. 이는 미분 가능하고, 따라서 딥러닝 모델로 학습될 수 있다. fully-convolutional하기 때문에 임의의 해상도에서 활용 가능하다.

# Generative Image Inpainting with Contextual Attention

![visualization of attention map](https://user-images.githubusercontent.com/59387983/87590836-683f0f00-c722-11ea-9ea1-2f69902acf6a.PNG)

Generative Image Inpainting with Contextual Attention라는 논문에서 발췌하였다.


컨브 뉴럴넷은 지역적인 합성곱커널로 레이어를 따라서 처리하기 때문에 어떤 멀리 떨어진 공간상의 위치로부터 특징을 반영하기에 효과적이지 않다. 이러한 한계를 극복하기 위해서 어텐션 메커니즘을 고려했고, 이를 novel contextual attention layer이라 명명한다.

# Match and attend

![contextual attention layer](https://user-images.githubusercontent.com/59387983/87590832-66754b80-c722-11ea-8eee-79858765ca14.PNG)

우리는 백그라운드를 둘러싸고 있는 누락된 픽셀들의 특징을 주변과 일치는 것을 고려하였다. 우선, 3x3 패치로 배경을 추출하고, 이를 컨벌루션 필터처럼 reshape한다. 그리고 포레그라운드 패치와 백그라운드 패치를 일치시키기 위해서 정규화된 내부 곱(cosine similarity)을 측정하였다. 그리고 포레그라운드 패치와 백그라운드 패치를 일치시키기 위해서 정규화된 내부 곱(cosine similarity)을 측정하였다. 그라운드와 포레그라운드 차원 사이의 scaled softmax로 유사도를 측정하였다. 각 픽셀의 attention score를 측정하기 위해서 λ를 softmax로 처리하기 전에 곱해주었다. λ는 어떤 고정된 값이다. 이는 convolution and channel-wise softmax로서 효과적으로 구현되었다. 이렇게 추출된 패치를 deconvolutional filters로 사용하여 포레그라운드를 재구성한다.

# Cosine Similarity
![cosine similarity original function](https://user-images.githubusercontent.com/59387983/87630870-22f5fe00-c771-11ea-8d2b-0c9c36e504f8.PNG)

코사인 유사도는 −1에서 1까지의 값을 가지며, −1은 서로 완전히 반대되는 경우, 0은 서로 독립적인 경우, 1은 서로 완전히 같은 경우를 의미한다.

텍스트 매칭에 적용될 경우, A, B의 벡터로는 일반적으로 해당 문서에서의 단어 빈도가 사용된다. 코사인 유사도는 문서들간의 유사도를 비교할 때 문서의 길이를 정규화하는 방법의 하나라고 볼 수도 있다.

정보 검색의 경우, 문서의 단어 빈도(tf-idf 가중치)가 음의 값이 되는 것이 불가능하므로 두 문서 사이의 코사인 유사도는 0에서 1까지의 값으로 표현되며, 두 단어 빈도간의 각도는 90°를 넘을 수 없다.

![cosine similarity](https://user-images.githubusercontent.com/59387983/87599945-b35f1f00-c72e-11ea-8dc8-b4ca755b6951.PNG)
  
