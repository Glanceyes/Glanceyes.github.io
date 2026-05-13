---
title: Attention 기법을 사용한 Seq2Seq with Attention
date: '2023-08-28 18:00:00'
featured_image: https://vip2.loli.io/2023/09/01/hYXlV3BMiqyDFub.jpg
published: true
---



> RNN 계열 모델인 LSTM을 여러개 이어서 encoder와 deocder로 만든 Seq2Seq에 관해 먼저 알아보고, 매 time step이 지날수록 이 Seq2Seq의 hidden state에 점차 많은 정보를 욱여넣게 되는 단점을 극복한 Seq2Seq with Attention에 관해 알아보고자 한다. 간단히 말하면 Seq2Seq with Attention은 decoder의 hidden state로 해당 time step에서의 결과를 내보낼 때, encoder의 어떠한 hidden state에 주목할지를 반영하여 해당 time step에서의 output을 내는 모델이다.



<br/>

## Seq2Seq



### Seq2Seq란?



Seq2Seq 모델은 RNN의 구조 중에서 many to many의 형태에 해당된다고 볼 수 있으며, 입력 sequence를 모두 읽은 후 출력 sequence를 출력 또는 예측하는 모델이다.

여기서 자연어 처리의 관점으로 볼 때 입력과 출력 sequence는 모두 단어 단위로 모인 문장이다.



<br/>



![img](https://vip2.loli.io/2023/09/01/pgZRKxHyVXlJe7N.jpg)



입력 문장을 읽어들이는 RNN 계열의 모델을 encoder로 부르고, 출력 문장을 순차적으로 단어를 생성하는 RNN 계열의 모델을 decoder로 부른다.

일반적인 RNN 계열의 모델처럼 매 time step 마다 encoder와 decoder 각자의 파라미터를 공유하지만, encoder와 decoder가 서로 파라미터를 공유하지는 않는다.

<br/>

위의 그림에서는 encoder와 decoder의 RNN 계열 모델로 LSTM을 채택한 것을 볼 수 있으며, encoder에서 마지막 time step에서의 hidden state $h_t$는 decoder의 첫 번째 time step의 입력으로 주어지는 hidden state인 $h_0$의 역할을 한다.



<br/>



![img](https://vip2.loli.io/2023/09/01/xkE8eB3QFvgZohX.jpg)



이때, 순차적으로 단어를 생성하는 decoder에서 첫 time step의 입력으로 넣어주는 단어를 특수문자로서 start token 또는 SoS(Start of Sequence)를 사용한다.

위의 그림에서는 표현이 생략되어 있지만, 기존의 LSTM과 마찬가지로 매 time step마다 이전 time step에서 구한 hidden state와 output으로 나온 결과를 input으로 넣는다.

또한 언제까지 단어를 생성해야 할지를 구하려면 문장이 끝나는 시점을 예측해야 하므로 출력 단어로 end token 또는 Eos(End of Sequence)가 나오는 순간까지를 최종적인 출력으로 구한다.



<br/>



## Attention 모듈을 사용한 Seq2Seq



### Attention 모듈을 사용하는 이유



![rnn_problem](https://vip2.loli.io/2023/09/01/wOKgQtlcux5MhIL.png)



위에서 살펴 본 Seq2Seq에서 추가적인 모듈로 attention을 활용할 수 있는데, 이 attention 모듈을 사용하려는 이유는 Seq2Seq 모델에서의 encoder와 decoder에 해당되는 RNN 계열 모델의 한계를 극복하기 위해서다.

Seq2Seq 모델에서 RNN 계열 모델을 사용하면 매 time step 마다 hidden state가 정보를 누적하면서 업데이트 되는데, hidden state의 차원은 고정되어 있으므로 긴 문장의 입력이 주어질 때 마지막 time step에서 출력으로 내보내는 hidden state에 문장의 전체 정보를 욱여넣을 수밖에 없다.



또한 LSTM을 통해 Vanilla RNN에서의 long-term dependency 문제를 해결했다고 하더라도 앞서 멀리 떨어진 time step에서의 정보는 RNN 모듈을 지나오면서 변질되거나 소실될 수 있다.

<br/>

<br/>



![lstm_problem](https://vip2.loli.io/2023/09/01/pREQNXLtI8eWSVm.png)



예를 들어, 자연어 처리에서 문장 태스크를 수행한다고 가정하면 문장에서 가장 처음으로 나오는 주어가 있는 길이가 긴 문장이 입력으로 주어지고 이를 encoder와 decoder를 통해서 해석한 단어를 예측해야 하는데, 맨 처음으로 주어지는 주어가 입력 문장에서 맨 앞에 위치하므로 encoder를 거쳤을 때 나오는 hidden state는 입력 문장의 맨 앞에 있는 주어의 정보를 충분히 갖지 못할 가능성이 있다.

이로 인해 decoder를 통해 생성된 해석 문장의 품질이 낮아질 수 있으며, 이를 해결하기 위해 차선책으로 입력 문장의 순서를 뒤집어서 초반에 등장하는 단어의 정보를 잘 갖도록 하는 방법도 제안되었지만 근본적인 해결책은 아니었다.





<br/>



### Attention 모듈을 Seq2Seq에 적용하는 방법



앞서 Seq2Seq 모델에서 decoder는 encoder의 마지막 time step에서 나온 hidden state에만 의존했지만, attention 모듈을 사용하면 encoder에서 매 time step 마다 나온 hidden state를 모두 decoder에 제공함으로써 decoder의 매 time step에서 적절한 단어를 예측할 때 선별적으로 필요한 hidden state를 사용할 수 있다.

예를 들어, LSTM 모델로 구성된 encoder에서 어떤 time step $t$까지 수행했을 때 나오는 hidden state인 $h_t$는 $t$에서의 입력 단어인 $x_t$의 정보를 더 많이 가질 수 있을 것이다.

이를 decoder의 time step $t$에서 encoder의 $h_t$를 활용할 수 있으면 해당 time step에서의 정보를 좀 더 많이 반영할 수 있게 된다.



꼭 $t$뿐만이 아니라 $t$와 근접한 time step에서의 정보를 더 많이 반영할 수도 있는데, 이를 attention 모듈을 통해 어떠한 time step에서의 hidden state를 많이 반영할 것인지를 구할 수 있다.





<br/>



### Seq2Seq with Attention 구조와 학습 과정



![img](https://vip2.loli.io/2023/09/01/hYXlV3BMiqyDFub.jpg)



Encoder에서는 Seq2Seq와 마찬가지로 입력 문장 단어를 순차적으로 받아서 각 time step에서의 hidden state를 구한다. 즉, hidden state vector를 구하는 과정은 LSTM과 동일하다. 또한 Seq2Seq처럼 encoder의 마지막 time step에서 나온 hidden state를 decoder의 첫 time step의 입력으로 사용한다.





Decoder의 첫 time step에서는 encoder의 마지막 time step에서의 hidden state를 $h_0$으로 받고 이와 함께 start token을 입력으로 받아서 현재 time step의 hidden state인 $h_1$를 구한다. 이도 마찬가지로 LSTM과 동일한 방식으로 구한다. 헷갈리면 안 되는 것이 hidden state를 매 time step 마다 업데이트하는 과정의 기본적인 베이스라인은 LSTM과 같은 방법으로 동작한다는 것이다.





이 $h_1$을 encoder에서 매 time step 마다 구한 hidden state와 모두 내적을 하여 attention score를 계산하여 어떠한 time step에서의 hidden state에 크게 주목할지 logit을 구한다.

<br/>



Attention score를 softmax를 통해 확률 분포로 나타내고, 각 hidden state의 비중이 확률로 주어지면 이를 가지고 hidden state를 가중평균하여 attention output을 구한다.





이를 decoder의 현재 time step에서의 hidden state인 $h_1$과 attention 모듈의 output인 context vector를 concatenation하여 output layer의 입력으로 들어가고, 이로부터 다음에 나올 단어를 예측하게 된다.



<br/>



![img](https://vip2.loli.io/2023/09/01/5EmKwaFlW19LVbn.jpg)



이러한 과정을 decoder에서 순차적으로 모든 time step을 거치면서 수행되며, Seq2Seq with attention 모델의 학습 목표는 decoder에서 각 time step에 attention 모듈을 사용하여 예측한 결과 문장이 실제 output 문장과 가까워지도록 그 오차를 줄이는 방향으로 학습하는 것이다.





<br/>



### Seq2Seq with Attention에서 각 파라미터의 역할



Decoder에서 각 time step별로 나온 hidden state는 Seq2Seq와 마찬가지로 해당 time step의 다음으로 올 단어를 예측하기 위한 용도이다.

그런데 Seq2Seq with attention 모델에서는 encoder의 모든 hidden state와 각각 내적을 취해줌으로써 어떠한 time step에서의 hidden state에 주목할지를 구하게 되는데, 이를 attention vector로 볼 수 있다.

이를 가지고 encoder의 모든 hidden state에 관한 가중평균을 계산하여 decoder의 해당 time step에서 encoder의 어떠한 hidden state를 좀 더 많이 반영할지를 알려주는 추가적인 output layer의 입력이 되는 것이다.



즉, decoder의 각 time step에서의 hidden state는 다음에 올 단어를 예측하는 역할도 하지만 동시에 encoder에서 어떠한 time step의 hidden state를 좀 더 많이 반영할지도 영향을 주는 파라미터이다.





<br/>



### Teacher Forcing



![img](https://vip2.loli.io/2023/09/01/XvZhDemL38Qxt7B.jpg)

Seq2Seq with attention 모델에서는 이전 time step에서의 예측 결과 단어를 다음 time step에서의 입력 단어로 주지 않고 GT(Ground Truth)에 해당되는 단어를 입력으로 준다.

이는 이전 time step에서 모델이 잘못된 단어를 예측한다 하더라도 다음 time step에서 올바른 단어를 예측할 수 있도록 보완하는 역할을 하는데, 이를 teacher forcing 방법이라고 한다.



Teacher forcing은 학습 속도를 높이고 학습 데이터에 관해 정확도를 높일 수는 있지만, 실제 테스트 환경에서 모델을 사용할 때는 괴리가 생길 수 있어서 teacher forcing이 항상 좋다고는 볼 수 없다.

그래서 이를 보완하고자 teacher forcing만으로 학습하는 배치를 먼저 진행하고 이후에는 teacher forcing을 하지 않는 학습 방식을 통해 teacher forcing의 단점을 해결하고자 하는 방법이 있다.





<br/>



## Attention Mechanism



### Attention Score을 계산하는 다양한 방법



Attention score를 구하는 방법에는 단순히 hidden state끼리 내적하는 것뿐만이 아니라 다양하게 계산할 수 있다.

<br/>



$$ \text{score}(h_t, \bar{h}_s) = \begin{cases} h_t^\top \bar{h}_s & \mbox{dot}\\ h_t^\top W_a \bar{h}_s & \mbox{general}\\ v_a^\top \tanh (W_a\left[h_t; \bar{h}_s \right]) & \mbox{concat} \end{cases} $$

<br/>



Seq2Seq with attention 모델에서 decoder의 time step $t$에서의 hidden state를 $h_t$, encoder의 임의의 time step에서의 hidden state를 $\bar{h}_s$라고 하면, 앞서 살펴 본 attention score 구하는 방식은 단순히 두 hidden state의 내적을 구하는 것이었다.

이를 선형대수 관점에서 해석하면 내적하는 행렬의 각 원소끼리 곱하여 합한 것으로 볼 수 있다.



여기서 더 나아가 두 hidden state를 내적할 때마다 사용하는 파라미터를 공유하여 내적을 좀 더 확장한 형태인 generalized dot product도 존재한다.



이는 선형대수 관점에서 두 행렬을 곱하는 식 사이에 항등 행렬과 같은 크기의 행렬이 곱해지는 것이고, 이는 내적하여 각 차원별로 원소를 곱할 때 가중치로 작용하는 역할을 한다.

<br/>

![attention_concatenation](https://vip2.loli.io/2023/09/01/SnrV2MHsLykia7C.png)



두 hidden state를 concatenation 하는 방법은 두 hidden state가 입력을 주어지고 이를 fully connected layer에 통과시켜서 나오는 벡터를 구하고, 여기에 비선형 활성함수를 통과시킨 후 또 한 번의 fully connected layer를 통과시켜서 최종적인 scalar 값을 구하는 것이다.

마지막 fully connected layer에서 $v_a$처럼 벡터 형태의 파라미터로 나타낸 이유는 이전 fully connected layer를 통과한 벡터를 가지고 scalar 값을 구해야 하므로 같은 차원의 벡터를 transpose한 벡터를 곱하는 것을 표현하기 위함이다.



<br/>



## Attention 모듈의 의의



### Information Bottleneck 해결



Seq2Seq 모델의 decoder에서 다음 time step에 올 output을 예측할 때 encoder의 time step에서 어느 부분의 hidden state에 주목할지를 구할 수 있으므로 긴 문장의 앞에 오는 부분의 정보 손실 문제에 관해서도 자유로운 장점이 있다.

이는 기존 Seq2Seq 모델에서 decoder가 encoder의 마지막 time step에서 나온 hidden state에만 의존하는 현상인 information bottleneck 문제를 해결한 것으로 볼 수 있다.



<br/>



### Gradient Vanishing Problem 예방



Attention 모듈을 사용하지 않으면 long-term dependency로 인해 매번 같은 파라미터가 곱해지면서 gradient가 사라지거나 매우 커지는 문제가 발생할 수 있는데, attention 모듈을 통해 모델 학습 과정에서 gradient vanishing problem을 해결할 수 있다.



<br/>



### Interpretability 향상



Attention 모듈을 사용하면 attention score를 가지고 확률 분포를 구한 것을 통해 해당 time step에서 decoder의 hidden state가 입력 문장의 어떠한 단어에 주목했는지를 값의 크기로 확인할 수 있어서 해석력을 지니게 된다.



<br/>



> **출처**
>
> 1. 네이버 커넥트재단 부스트캠프 AI Tech NLP Track 주재걸 교수님 기초 강의



> **업데이트**
>
> 1. Seq2Seq with Attention 그림에서 설명이 부족하거나 잘못된 부분을 수정했습니다. (2023. 03. 17)
