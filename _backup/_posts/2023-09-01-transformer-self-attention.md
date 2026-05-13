---
title: Transformer의 Self-Attention에 관한 소개와 Seq2Seq with Attention 모델과의 비교
date: '2023-09-01 19:00:00'
featured_image: https://vip2.loli.io/2023/09/02/HlEsjucNhikZeBK.jpg
published: true
---



>Transformer를 이해하려면 Seq2Seq with Attention 모델이 나오게 된 배경과 그 방법을 이해하는 것이 필요하다. 특히 transformer의 self-attention에 관해 한줄로 요약하면, Seq2Seq with Attention에서 decoder의 hidden state와 encoder의 hidden state를 구하는 과정에서 LSTM을 빼 버리고 이를 병렬적으로 처리하는 대신에 hidden state의 attention을 구하는 데 필요한 hidden state를 역할에 따라서 서로 다른 벡터로 구성하여 학습을 수행하는 방법이다. 비문이어서 이해하기 어려울 수 있지만 Seq2Seq with Attention에 관한 이해가 선행되면 transformer의 self attention도 이해하는 데 큰 어려움이 없을 것이다.



<br/>



## Transformer란?



Transformer는 'Attention is all you need' 논문에서 본격 제안된 모델이며, 자연어를 입력 받아서 또 다른 자연어를 출력하는 seq2seq 모델을 개선한 것이다.

기존에는 attention 모듈을 LSTM 또는 GRU를 기반으로 하는 seq2seq 모델의 애드온으로 사용되었지만, transformer를 통해 sequence 데이터를 처리할 때 필요로 하는 RNN 모델을 아예 attention 모듈로 교체할 수 있는 관점이 제안되었다.



<br/>



### Self-Attention에서 Seq2seq with attention과의 비교



Transformer를 본격적으로 분석하기 전에 seq2seq with attention을 다시 복습할 필요가 있다.

[Seq2Seq with Attention 글 보러가기](/blog/seq2seq-with-attention)



<br/>



Seq2seq with attention에서는 encoder에서 매 time step 마다 이전 time step에서의 hidden state $h_{t-1}$와 현재 time step에서의 input $x_t$를 가지고 현재의 hidden state인 $h_t$를 만들어 내고, 이를 통해 decoder에서 각 time step에서의 hidden state와 encoder의 모든 time step의 hidden state의 내적을 구하여 encoder에서 어떤 time step에서 등장한 단어에 집중할지를 결정한다.

'Attention is all you need'에서 소개된 transformer는 seq2seq with attention 모델의 매커니즘과 유사하므로 둘을 비교하면서 살펴 볼 필요가 있다.





![img](https://vip2.loli.io/2023/09/02/snRhrfp5AbS3V9C.jpg)





입력으로 input vector가 주어지면 우리는 input sequence의 전체 내용을 잘 반영하여 각 단어를 인코딩한 결과를 출력으로 내야 한다.



<br/>



$$ \left\{ x_1, x_2, x_3, x_4 \right\} = \left\{ \text{I}, \text{know}, \text{your}, \text{sacrifices} \right\} $$



<br/>





![img](https://vip2.loli.io/2023/09/02/Ys453WVOb8oDMeF.jpg)



단어 $x_1$인 'I'에 관한 임베딩 벡터를 만든다고 가정하면, 'I'에 대한 input vector가 마치 seq2seq with attention의 decoder에서 현재 time step에서의 hidden state $h_1$와 같은 역할을 하게 되며, 이는 우리가 찾고자 하는 정보를 나타내고 있는 벡터로 볼 수 있다.



더 나아가 seq2seq with attention 과정을 다시 살펴보면, encoder에서 만들어진 hidden state vector의 set을 가지고 decoder의 $h_t$와 내적을 수행해서 유사도를 구하고, 여기에 softmax를 취해서 encoder의 각 hidden state에 관한 확률 분포를 생성한다.

이를 가지고 encoder의 hidden state의 가중치로 고려해서 이를 가중 평균하여 decoder의 현재 time step에 관한 최종적인 attention 모듈의 context vector를 계산한다.



<br/>



![img](https://vip2.loli.io/2023/09/02/DFV6t7ckP58lGxw.jpg)





다시 돌아와서 transformer 과정을 살펴보면, 각 단어에 관한 임베딩된 input vector를 seq2seq with attention에서 decoder의 각 time step에 관한 hidden state로 고려할 수 있다.

그런데 여기서 transformer에서 주어진 각 단어에 관한 input vector도 마찬가지로 seq2seq with attention에서 encoder의 hidden state vector의 set으로 고려할 수 있다.



<br/>



이러한 관점에서 살펴보면 transformer에서 각 단어에 관한 input vector는 seq2seq with attention의 decoder와 encoder의 hidden state vector와 같은 역할을 하고, attention 모듈 계산 과정을 거치는 것도 seq2seq with attention에서 encoder와 decoder처럼 별개의 hidden state를 사용하지 않는다.

<br/>





![img](https://vip2.loli.io/2023/09/02/FK2eXlZsNB6xHbj.jpg)





그래서 결과적으로 동일한 input vector set 내에서 내적과 가중평균의 attention 계산 과정이 이루어진다는 의미에서 이를 'self-attention' 모듈이라고 부른다.



<br/>





![img](https://vip2.loli.io/2023/09/02/iGw1nT8quxDveR5.jpg)



그러나 위에서 설명한 것처럼 input vector set을 그대로 가지고 내적을 계산하게 되면 현재 보고 있는 단어인 자기 자신에 관해 내적을 수행한 유사도가 서로 다른 벡터와의 유사도를 구한 것보다 일반적으로 큰 값을 갖는 경향을 띠게 된다.

이렇게 될 경우 출력으로 내보낸 인코딩된 단어 vector도 자기 자신에 관한 원래 정보를 더 많이 포함되도록 나올 것이다.



그래서 이러한 기본적인 형태의 attention 모듈 작동 과정을 사용한 것에서 그치지 않고 이를 유연하게 확장하여 위에서의 문제를 개선하고자 하는 모델이 바로 'Attention is all you need'에서 제안된 transformer이다.



<br/>



## Self-Attention의 학습 과정



앞서 살펴본 내용에 의하면 주어진 입력에서 각 단어에 대응되는 동일한 input vector가 attention 모듈 계산 과정의 각 단계마다 서로 다른 역할을 하는 것을 알 수 있었다. 그런데 이는 앞서 정리한 바처럼 자기 자신에 관한 attention을 높이게 하는 단점을 지니고 있다. 그래서 이 논문에서는 각각의 역할을 하는 vector들을 input vector로 모두 동일하게 사용하는 방법이 아니라, 역할마다 서로 다른 벡터 query, key, value vector를 사용하여 학습을 수행하는 방법을 제시했다.

<br/>





![img](https://vip2.loli.io/2023/09/02/HlEsjucNhikZeBK.jpg)



예를 들어, 단어 $x_1$인 'I'에 관해 임베딩된 input vector가 마치 seq2seq with attention의 decoder에서 현재 time step에서의 hidden state $h_t$와 같은 역할을 하게 되어 attention 모듈의 element-wise product(내적) 계산 과정에서는 자기 자신과 다른 단어 vector들 간의 유사도를 구하는 용도로 사용된다.

<br/>





![img](https://vip2.loli.io/2023/09/02/twE2UBsFC6vkh41.jpg)



이는 주어진 입력 벡터에 관해 어떠한 벡터를 중요하게 선별하여 반영할지에 관한 그 기준이 되는 벡터로 사용되는데, transformer에서는 여기서 각 단어에 대응되는 query vector를 만들어 사용한다.



<br/>





![img](https://vip2.loli.io/2023/09/02/kiaUuSNeFOoTwm6.jpg)



또한 attention 모듈에서 query vector와의 유사도를 구할 때 언급한 각 단어의 input vector를 그대로 사용하지 않고 각 단어마다 key vector라는 별개의 vector를 만들어 사용하며, 이는 seq2seq with attention의 encoder의 hidden state vector set처럼 query vector와 내적이 될 때 재료가 되는 역할을 한다.



<br/>





![img](https://vip2.loli.io/2023/09/02/5NdOgr7F6UA1pTa.jpg)





그 다음에 query vector와 key vector 간의 유사도를 구한 것 중에서 어떠한 key vector와 내적할 때 높은 유사도를 지니는지 구한 결과에 softmax를 취하여 최종적으로 어떠한 단어에 주목할지에 관한 가중치를 얻는다.



Transformer에서는 여기서도 Attention 모듈에서와는 달리 가중 평균을 구할 때 각 단어별로 임베딩된 input vector를 그대로 사용하지 않고 각 단어별 value vector를 사용한다.

이를 통해 가중치를 가지고 value vector를 가중 평균한 결과 vector인 $h_1$가 단어 $x_1$에 관해 인코딩된 output vector이다.



<br/>





![transformer2](https://vip2.loli.io/2023/09/02/dIB1jfntsOeLQ6A.png)



한 sequence를 attention을 통해 인코딩 하는 과정에서 입력으로 주어진 각 단어의 vector들이 query, key, value vector의 역할을 수행할 수 있도록 여러 벡터로 변형되며, 각 역할에 따라 서로 다른 벡터의 형태로 선형 변환될 수 있도록 별도의 $W^Q$, $W^K$, $W^V$ 파라미터를 추가로 정의한다.



<br/>





![transformer3](https://vip2.loli.io/2023/09/02/PykrtsJjwaDgeTQ.png)



그래서 단어별로 모든 역할에 관해 제한적으로 동일한 벡터를 사용하지 않고 역할에 따라 query, key, value vector로 쓰일 때 다른 형태로 사용될 수 있도록 했으며, 동일한 단어에 관한 input vector로부터 변환된 query와 key vector를 내적해도 반드시 유사도가 가장 높은 값이 나오지는 않는 유연성의 효과를 얻는다.

이러한 transformer의 특징으로 인해 모델의 입력으로 주어지는 단어의 개수만큼 각각 key, value vector를 갖게 된다.

이론적으로 반드시 query vector 수가 단어의 수와 같아야 하는 것은 아니지만, 실제 transformer 구현상에서는 query vector도 입력으로 주어진 단어의 수만큼 갖는다.



<br/>





![img](https://vip2.loli.io/2023/09/02/9V1nmxupSLDCzdb.jpg)



이처럼 단어 $x_1$뿐만이 아니라 $x_2$, $x_3$에 관해서도 $W^Q$ 변환을 통해 query 벡터를 구하고, 앞서 사용한 key vector와 value vector를 가지고 같은 attention 계산 과정을 거친다.



거시적인 관점에서 바라보면 임베딩된 각 입력 단어 vector이 한 행에 해당하는 하나의 행렬 $X$로 볼 수 있고, 선형 변환으로 사용되는 파라미터 $W^Q$, $W^K$, $W^V$를 각각 또 다른 행렬로 생각하면 위의 그림과 같은 행렬의 곱으로 이해할 수 있다.





<br/>



## Self-Attention의 의의



각 단어가 transformer를 통해 인코딩되는 과정에서의 흐름을 분석해 보면, 기존의 RNN 기반 모델처럼 어떤 한 방향성을 띠지 않고 각 단어가 인코딩될 때 query와 key vector를 가지고 유사도를 구하여 이를 value vector의 가중 평균을 구하는 가중치로 사용되는 과정에서 매 단어에 관한 인코딩 출력 결과를 낼 때마다 모든 단어를 참고하게 되는 의존성을 지닌다.



그래서 sequence의 길이가 길더라도 self-attention 모듈을 적용하여 sequence 내 각 단어를 인코딩 벡터로 만들게 되면, 현재 보고자 하는 단어와 이의 인코딩 벡터를 구할 때 참고하는 단어 사이의 time step이 굉장히 멀다고 하더라도 현재의 query vector와 참고하는 단어의 key vector 간의 유사도가 높으면 해당 단어에 관한 정보를 손쉽게 가져올 수 있는 장점이 있다.



이는 기존의 RNN 기반 모델에서 time step이 멀어질수록 앞에 등장한 단어의 정보가 hidden state를 계속 업데이트 하는 과정에서 소멸되거나 매우 적어지고, hidden state 하나의 벡터에 이전 정보를 모두 욱여넣어서 발생하는 병목 현상을 해결한 것이다.





<br/>



## Dot-Product Attention



입력으로 주어진 어떤 한 단어의 query vector $q$가 있고, key vector $k$와 value vector $v$의 쌍으로 이루어진 다수의 $(k, v)$ 쌍이 존재한다.



주어진 단어의 인코딩된 출력 벡터는 value vector의 가중 평균이며, 가중 평균에서 쓰이는 각각의 가중치는 query vector $q$와 각 단어에 대응되는 key vector $k$의 내적에 의한 유사도 값이다.



Query vector와 key vector는 element-wise product인 내적 연산이 이루어져야 하므로 같은 차원인 $d_k$를 지녀야 하고, value vector의 경우에는 query와 key vector의 차원과는 꼭 동일하지 않아도 되므로 attention 모듈이 실행되는 과정에는 아무런 문제가 없으므로 $d_v$ 차원을 지닌다고 볼 수 있다. 단, query vector와 key vector는 단어 token 간의 연관성을 나타내는 attention을 구해야 하므로 query vector와 key vector의 개수는 단어의 token 개수와 동일해야 한다. 마찬기자로 value vector의 수도 단어의 token 개수와 같아야 한다.

<br/>



$$ \text{softmax}(y_i) = \frac{\exp(x_i)}{\sum_j \exp(x_j)} $$

<br/>



$$ \text{weight}_{i} = \frac{\exp (q \cdot k_i)}{\sum_j \exp(x_j)} $$

<br/>



위의 수식은 softmax의 공식을 사용해서 한 단어에 대응되는 query vector와 각 단어에 관한 key vector와의 내적을 구하는 가중치를 구하는 식이다.

<br/>



$$ A(q, K, V)= \sum_i \frac{\exp (q \cdot k_i)}{\sum_j \exp(q \cdot k_j)} v_i = \sum_i \text{weight}_i \cdot v_i $$



<br/>



위에서 구한 가중치를 가지고 현재 보고 있는 단어 $q$에 관한 인코딩된 출력 벡터를 구하기 위해 각 단어에 대응되는 value vector의 가중 평균을 구하는 과정을 나타낸 식이다.



이를 모든 단어에 관한 query vector를 하나의 행렬로 합쳤을 때 구하는 식으로 나타내면 다음과 같이 정리할 수 있다.



<br/>



$$ (|Q| \times d_k) \times (d_k \times |K|) \times (|V| \times d_v) = (|Q| \times d_v) $$



<br/>





$$ A(Q, K, V) = \text{softmax}(Q \cdot K^T) V $$



<br/>



위의 수식에서의 softmax는 row-wise softmax이며, query와 key matrix를 행렬 곱 연산한 결과 행렬에서 각 행의 원소의 합이 1이 되도록 만드는 역할을 한다.

<br/>





![img](https://vip2.loli.io/2023/09/02/kPWDclLvwJtMz1C.jpg)





$(\lvert Q\rvert \times d_k) \times (d_k \times \lvert K\rvert)$ 식에서는 한 query row vector와 다른 key column vector 간의 행렬 곱을 통해 마치 query와 key vector를 내적하는 것처럼 작동한다.

이를 통해 구한 $(\lvert Q \rvert \times \lvert K \rvert)$ 행렬에서 각 row vector는 해당 query vector 단어에 관해 각 key vector와 유사도를 구한 가중치가 되고, 이를 $(\lvert Q \rvert \times \lvert K \rvert ) \times (\lvert V \rvert \times d_v)$ 식을 통해 모든 value row vector와 행렬 곱 연산을 적용한다.

<br/>



앞에서 언급한 바처럼 key와 value vector 수는 항상 같으므로 $\lvert K \rvert$와 $\lvert V \rvert$는 같은 값을 지니고, 여기서 추가로 주목해야 할 점이 이 계산 과정에서 행렬 곱이 마치 value vector의 각 원소에 가중치를 모두 곱하고, 가중치가 곱해진 모든 value vector를 각 원소별로 합하는 과정으로 흘러간다는 것이다.



이론상으로는 $d_k$와 $d_v$가 달라도 되지만, 실제로 transformer에서는 query, key, vector matrix는 동일한 shape를 지니도록 하므로 $\lvert Q \rvert = \lvert K \rvert = \lvert V \rvert$와 $d_k = d_v$를 만족하도록 한다.





<br/>



### Scaled Dot-Product Attention



<br/>



$$ A(Q, K, V) = \text{softmax}(\frac{Q \cdot K^T}{\sqrt{d_k}}) V $$



<br/>



특정 query vector와 각각의 key vector를 내적하는 과정에서 원소끼리 곱한 값을 더할 때 값이 너무 커지는 현상을 예방하고자 $\sqrt{d_k}$로 나눠서 정규화하는 과정을 거친다.

그런데 query와 key vector 내적 값은 1차원의 scalar로 나오지만, 이를 계산하는데 참여하는 query와 key vector의 차원 수는 $d_k$로 이보다 더 큰 경우가 많다.



<br/>



![transformer5](https://vip2.loli.io/2023/09/02/7PzR5IfOb3cdait.png)



통계적으로 query vector와 key vector의 각각의 원소 값을 특정한 평균과 분산을 가지는 확률 변수로 고려하면 이를 이해할 수 있다.

예를 들어 query vector인 $(a, b)$와 key vector인 $(x, y)$를 내적을 하고, 각 독립 확률 변수 $a$, $b$, $x$, $y$는 평균이 0이고 분산이 1인 확률 분포를 갖는다고 가정한다.

그러면 내적의 결과는 $ax + by$인데, 여기서 각 확률 변수의 평균은 모두 0이므로 새로운 확률 변수인 $ax + by$의 평균은 0이 된다.

또한 각 확률 변수의 분산은 모두 1이므로, 알려진 통계 증명에 따라서 새로운 확률 변수인 $ax + by$의 분산은 1 + 1인 2가 된다.

<br/>





그런데 만약 query vector와 key vector의 차원 수가 매우 커진다면 분산 자체가 매우 커질 수 있음을 예상해볼 수 있다.

그래서 query와 key vector의 내적을 query와 key vector의 차원 수인 $d_k$로 나눠줘서 각 key vector에 관한 유사도 확률 분포가 고르게 나타날 수 있도록 하기 위함이다.

이는 value vector를 가지고 가중 평균을 구할 때 $d_k$의 크기에 영향을 덜 받게끔 하여 반영할 정보가 한쪽 단어로만 몰리지 않고 더 많은 단어가 고르게 반영되도록 할 수 있다.

또한 한쪽 단어의 반영 값이 너무 커져서 단어의 확률 분포가 불균형해지면 back propagation 과정에서 매우 적게 반영되는 단어에 관한 gradient가 사라지는 현상이 발생할 수 있는데, dot-product attention에 scaling을 적용함으로써 이를 예방할 수 있다.



<br/>



> **출처**
>
> 1. 네이버 커넥트재단 부스트캠프 AI Tech NLP Track 주재걸 교수님 기초 강의



> **업데이트**
>
> 1. Transformer와 Seq2Seq with Attention 그림에서 잘못 설명되거나 누락된 부분을 수정했습니다. (2023. 03. 17)
