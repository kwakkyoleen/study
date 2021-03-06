---
title: "신경망과 딥러닝_본론"
date: 2019-12-28T03:21:43+09:00
draft: true
html header: 
---

## 로지스틱 회귀

요약하자면 출력 레이블 y가 0 또는 1로 결정되는 이진분류를 하기위해 사용되는 알고리즘이다.

수식을 사용한 조금 더 재미있는 설명은 다음과 같다.

주어진 \\(x\\)를 이용하여 \\(y\\)의 예측값 \\(\hat{y}=P(y=1|x)\\)를 구하려고 할 때 \\( x\in \mathbb{R}^{n_{x}} \\)이고 로지스틱 회귀의 파라미터 \\(w\\)와 \\(b\\)는 각각 \\( w\in \mathbb{R}^{n_{x}} \\), \\( b\in \mathbb{R}\\) 이다. 이때 예측값 \\(\hat{y}\\)를 구하기 위한 가장 보편적인 방법은 \\(\hat{y}=w^{T}x+b\\)이다. 하지만 이진분류를 하기 위해서는 \\(\hat{y}\\)의 값이 ( \\(0<=\hat{y}<=1\\) )로 고정되어야 하기 때문에 \\(\hat{y}\\)의 값을 한정시켜 주기 위해 시그모이드 함수 \\(\sigma (z)\\)를 사용한다.

![Example image](https://cphinf.pstatic.net/mooc/20180605_88/1528177200744k2iiP_PNG/sigmoid.PNG)

\\(\sigma (z)=\frac{1}{1+e^{-z}}\\)이며 이 함수의 개형은 대략 위 그림과 같으므로 \\(\hat{y}=\sigma(w^{T}x+b)\\)로 \\(\hat{y}\\)을 한정시켜 로지스틱 회귀를 구한다.

<br>

## 손실함수와 비용함수

주어진 \\(X\\)와 \\(Y\\)를 이용해 \\(\hat{y}^{(i)} \approx y^{(i)} \\)가 되도록 로지스틱 회귀의 매개변수 \\(w\\)와  \\(b\\)를 학습시키기 위해 손실함수와 비용함수를 정의해야 한다. 손실함수는 알고리즘이 출력한 예측값 \\(\hat{y}\\)와 실제값 \\(y\\)의 오차를 정의하는 함수이며 보통은 \\(L(\hat{y},y)=\frac{1}{2}(\hat{y}-y)^{2}\\)로 정의한다. 하지만 이렇게 정의된 손실함수는 로지스틱 회귀를 전역최소가 아닌 지역최소에 빠트릴 수 있기 때문에 잘 쓰이지 않으며 대신 \\(L(\hat{y},y)=-(y\mathrm{log}\hat{y}+(1-y)\mathrm{log}(1-\hat{y}))\\)로 정의한다. 이 함수는 \\(y=0\\) 인경우 \\(\hat{y}\\)가 0에 가까워질수록 0에 수렴하게 되며 \\(y=1\\) 인경우 \\(\hat{y}\\)가 1에 가까워질수록 0에 수렴하게 된다. 즉 \\(\hat{y}\\)가 \\(y\\)에 가까워질수록 최솟값인 0에 수렴하게 된다. 비용함수는 모든 입력에 대한 손실함수의 평균값을 정의하는 함수이며 식으로 나타내면 \\(J(w,b)=-\frac{1}{m} \sum_{i=1}^{m}(y^{(i)}\mathrm{log}\hat{y}^{(i)}+(1-y^{(i)})\mathrm{log}(1-\hat{y}^{(i)}))\\)로 쓸 수 있다. 따라서 신경망을 목적을 이룰 수 있도록 학습하기 위해서는 비용함수가 가장 적어지는 방향으로 학습을 시켜야 한다. 손실 함수의 증명은 뒷부분에 추가할 예정이다.

## 경사하강법

경사 하강법은 마치 비용함수의 경사를 타고 내려오듯이 \\(\hat{y}\\)와 \\(y\\)의 값의 차가 가장 적어지는 비용함수의 최솟값을 찾아 최적의 \\(w\\)와 \\(b\\)를 구하는 방법이다.

![비용함수](/beeyong.PNG)

위 그림은 아래가 볼록한 로지스틱 회귀에 가장 적합한 비용함수\\(J(w,b)\\)의 그래프다. 만약 아래가 볼록하지 않고 울퉁불퉁 하다면 부분 최소가 여러개이기 때문에 비용함수는 전역최소에 수렴하지 않고 부분최소에 납치당할 것이다. 비용함수의 최솟값을 찾기 위해서는 우선 적절한 \\(w\\)와 \\(b\\)를 설정해야한다. 위 비용함수 같은 경우에는 어디에서 초기화를 하든 대부분의 경우 거의 같은 점으로 수렴하기때문에 보통 초깃값을 0으로 설정한다. 초기값이 설정되면 가장 가파른 내리막으로 한단계씩 이동하며 최솟값을 찾는다. 이를 간단하게 수식으로 표현하면 \\(w^{(i+1)}=w^{(i)}-\alpha \frac{dJ(w)}{dw}\\)으로 나타난다. 이때 \\(\alpha\\)는 학습률이며 신경망의 학습 속도를 조절하는 역할을 한다. 이때 활성화 함수로 시그모이드함수를 쓴다고 가정하고 \\(\frac{dL}{dw}\\)를 구한다면 미분방정식을 사용하여 \\(\frac{dL}{dw} = \frac{dJ}{da}\frac{da}{dz}\frac{dz}{dw} \\)로 나누어 표현할 수 있고  \\(L(a,y)=-(y\mathrm{log}a+(1-y)\mathrm{log}(1-a))\\) (\\(\hat{y}=a\\))이므로 \\(da = \frac{dL}{da}=-\frac{y}{a}+\frac{1-y}{1-a}\\), \\(dz = \frac{dL}{da}\frac{da}{dz}=a-y\\), \\(dw=\frac{dL}{dw} =xdz\\)이며 \\(db\\)의 경우 또한 \\(db=\frac{dL}{db}=dz)가 성립한다. 이때 만약 샘플이 m개라면 행렬식을 사용하여

![기울기](/giulgi.PNG)

위 처럼 구할 수 있다.

## 벡터화

위 식을 계산하기 위해 for처럼 명시적인 반복문을 쓰게되면 매우 속도가 느려진다. 따라서 for문 대신에 numpy를 사용하여 행렬을 직접 계산한다. 예를들어 다음 코드는 이렇게 간략화 시킬 수 있다.

벡터화를 사용하지 않은 경우
```python
z = 0
for i in range(nx):
    z += w[i] * x[i]
z += b
```

벡터화를 사용한 경우
```python
z = np.dot(w,x) + b
```

numpy 주요 함수 설명

* `numpy.dot(a, b)` 는 a와 b의 행렬곱을 구해주는 함수
* `numpy.zeros(a,b)` a*b크기의 빈 행렬을 만드는 함수
* `m.reshape(a,b)` 행렬 m의 크기를 a*b로 재정의해주는 함수
* `m.shpae` 행렬 m의 형태를 볼 수 있는 함수(행, 열)
* [나머지 여기 참조](https://docs.scipy.org/doc/numpy-1.13.0/reference/routines.math.html)

m개의 훈련 샘플을 벡터화를 사용하여 로지스틱 회귀를 계산하면 다음과 같다.
```python
Z = np.dot(np.transpose(w),X) + b
A = sigmoid(Z)
```
이때 대문자 알파벳은 여러개의 훈련 샘플을 포함하고 있음을 의미한다. 이에 이어 경사 하강법의 코드를 작성하면
```python
dZ = A - Y
dw = dot(X,np.trnaspose(dZ))/m
db = np.sum(dZ)/m
w = w - alpha*dw
b = b - alpha*db
```
인데 위 코드에서 한가지 모순점이 있다. Z벡터와 b의 차원은 분명히 다른데 계산이 되고있다. 이는 파이썬의 브로드캐스팅 덕분에 가능한 것이다.

## 브로드캐스팅

브로드 캐스팅은 연산 대상인 두 행렬의 차원이 맞지 않을 때 특정 조건을 만족하면 뒤 행렬의 차원을 알맞게 바꾸어 준다. 만약 (4,1)인 a와 상수 b를 곱한다 가정하면 b를 크기가 모두 b인 (4,1)행렬로 바꾸어준다. 행과 열이 반대인 경우도 마찬가지다. 심지어 (4,3)인 a와 (1,3)인 b를 연산할 때에도 b의 1행의 데이터를 복제하여 b를 (4,3) 만들어 연산시킨다. 이도 마찬가지로 행과 열이 반대인 경우도 적용된다.

## 비용함수 및 손실함수의 증명

앞의 내용에서 \\(y\\)값이 1이 될 확률은 \\(\hat{y}\\)이고 0이 될 확률은 \\(1-\hat{y}\\)라고 앞의 비용함수 및 손실함수 파트에서 언급했었다. 이 두가지 경우를 모두 한 수식에서 나타내면 다음과 같다.

\\(P(y|x)=\hat{y}^{y}(1-\hat{y})^{(1-y)}\\)

이는 결과가 이진분류이기때문에 가능한 수식이며 \\(y\\)가 0 또는 1일때의 이 수식은 앞서 언급한 수식과 동일한 결과를 가진다. 이 식에 \\(\mathrm{log}\\)를 취하고(로그 함수는 강한 단조증가 함수이기 때문에 로그를 취하지 않은 식을 최대화 하는것과 취한 식을 최대화 하는 것이 같은 결과를 얻는다) 아래로 볼록한 함수로 만들기 위해 -1을 곱해주면 손실함수 \\(L\\)과 일치하게 된다. 이를 모두 더한 후 평균을 내면 비용함수가 된다. 

## 신경망 네트워크

신경망은 입력을 받고 바로 출력을 하는 로지스틱 회귀와는 다르게 입력층, 은닉층, 출력층이 나뉘어져 있고 그중 은닉층의 유무가 가장 큰 차이점이라고 할 수 있다. 또한 은닉층은 겉으로는 보이지 않으며 각 층의 파라미터 및 매개변수들은 입력층부터 시작하여 \\(a^{[0]}\\), \\(a^{[1]}\\)... 같은 표기를 사용한다. 한 층 안에 여러개의 유닛이 존재할 때 l층의 n번째의 유닛은 \\(a^{[l]}_{n}\\)으로 표기한다. 신경망 층의 갯수를 셀 때 입력층은 제외하고 센다.

## 신경망에서의 정방향 전파

m개의 훈련 샘플을 벡터화 하여 전파시킨다면

```python
Z[1] = np.dot(np.transpose(W[1]),X) + b[1] #X = A[0], w는 계산을 위해 전치
A[1] = g[1](Z[1]) #g는 활성화 함수
Z[2] = np.dot(np.transpose(W[2]),A[1]) + b[2]
A[2] = g[2](Z[2])
....
```
이며 이를 층의 갯수가 k인 신경망에 더 간단히 적용시키면
 ```python
 for l in range(k)
    Z[l] = np.dot(np.transpose(W[l-1]),X) + b[1] 
    A[1] = g[l](Z[l])
```
가 된다.

## 활성화 함수

활성화 함수에는 sigmoid 만이 있는 것이 아니다. 오히려 단점이 많은 sigmoid는 이진분류의 출력레이블에서밖에 거의 쓰이지 않고 다른 활성화 함수들이 많이 쓰인다.

* sigmoid : \\(a=\frac{1}{1+e^{-z}}\\) z가 0일때 0.5의 출력을 가지며 이때문에 이진분류를 제외하고는 거의 모든 부분에서 tanh에 밀리는 함수. 요즘엔 많이 안쓰이다고 한다. 미분형 : \\(g'(z) = \frac{d}{dz}g(z) = g(z)(1-g(z))\\)
* tanh : \\(a=\frac{e^{z}-e^{-z}}{e^{z}+e^{-z}}\\) z가 0일때의 출력과 평균값이 0이며 나머지 부분은 sigmoid와 매우 비슷한 함수. 이녀석 마저도 최근엔 ReLu에 밀린다고 한다. 미분형 : \\(g'(z) = 1-(g(z))^{2}\\)
* ReLu : \\(a=max(0,z)\\) z가 0 이하일때는 기울기가 0이다가 그 이상 올라가면 기울기가 1이되는 함수. z가 0보다 클때 기울기가 1이므로 타 활성화 함수보다 학습이 빠르다고 한다. 미분형 : \\(g'(z) = 0\\) (z < 0), \\(g'(z) = 1\\) (z > 0)
* leakyReLu : \\(a=max(0.01z,z)\\) z가 0 이하일때 매우 조금의 기울기를 부여한 ReLu를 변형시킨 함수. ReLu보다 좋다는 사람도 있지만 그렇게 많이 쓰이진 않는다 한다. 미분형 : \\(g'(z) = 0.01\\) (z < 0), \\(g'(z) = 1\\) (z > 0)

![활성화](/plot0.PNG)

위의 예시에서도 볼 수 있듯이 활성화 함수로는 선형함수를 거의 쓰지 않는다. 특히 은닉층에서 활성화 함수를 쓰는 경우는 그냥 없다고 봐도 무방하다. 그 이유는 수학적으로 조금만 생각해 보면 알 수 있는데 활성화 함수로 선형함수를 사용하면 아무리 은닉층을 쌓아도 1차식 그대로이기 때문에 은닉층을 쌓는 의미가 없어져 버린다. 따라서 은닉층에는 반드시 비선형 활성화 함수를 사용한다.

## 신경망에서의 역방향 전파

![역전2](/go3.png)

경사 하강법과 비슷한 방법으로 역전파를 진행한다. 다만 제 2층에서 1층으로 넘어갈 때 \\(da^{[1]}\\)를 넘겨주어 다시 편미분을 진행한다.
\\(dz^{[2]}\\) 까지 한꺼번에 계산하여
\\(dz^{[2]}=a^{[2]}-y\\)
이고 \\(x^{[2]} = a^{[1]}\\)이므로
\\(dW^{[2]}=dz^{[2]}a^{[1]T}\\), 
\\(db^{[2]}=dz^{[2]}\\)

\\(da^{[1]} = W^{[2]T}dz^{[2]}\\)이고 이것으로 \\(dz^{[1]}\\)를 구하면 그 이후의 W와 b도 다 구할 수 있다. 이것을 점화식으로 만들면

\\(da^{[l-1]} = W^{[l]T}dz^{[l]}\\)  
\\(dW^{[l]}=dz^{[l]}a^{[l-1]T}\\)  
\\(db^{[l]}=dz^{[l]}\\)

![역전파](/go.png)

m개의 훈련 샘플에 대한 벡터화된 코드는 아래와 같다.
```python
dZ[2] = A[2] - Y
dW[2] = np.dot(dZ[2],np.transpose(A[1]))/m #비용함수의 도함수기 때문에 1/m이 붙음
db[2] = np.sum(dZ[2], axis = 1, keepdims = True)/m # axis = 0일땐 세로로 더하고 1이면 가로로 더함.
dZ[1] = np.dot(W[2],dZ[2])*g(미분)[1](z[1]) #요소별 곱
# 이하동일
```

또한 초기 가중치를 설정할 때 로지스틱 회귀와 같이 모든 파라미터를 0으로 설정한다면 모든 가중치가 똑같아 역전파시 모든 유닛이 똑같은 노드로 취급되어 학습이 이루어지지 않기 때문에 가중치를 랜덤으로 부여해야한다.

```python
w = np.random.randn(행,열)
```

## 심층 신경망의 학습과정

보편적으로 다음과 같은 과정을 통해 이루어진다,

0. 간단한 특징 찾기
0. 탐지된 것들을 모아 복잡한 특징 찾기
0. 복잡한 것들을 모아 더 복잡한 특징 찾기
0. 계속 찾기
0. 문제 해결

따라서 신경망이 깊으면 깊을수록 복잡한 것을 잘 처리하므로 더욱 잘 작동하게 된다. 또한 은닉층의 갯수가 너무 작을시 작은 은닉층으로 복잡한 일을 처리해야하기 때문에 한 은닉층 안에 매우 많은 유닛이 생성될 수 도 있다.

## 변수와 하이퍼파라미터

변수는 학습 가능한 w와 b 등을 뜻하고 하이퍼 파라미터는 다음과 같은 다양한 것들을 가르킨다.

* 학습률(알파)
* 반복 횟수
* 은닉층의 갯수
* 은닉 유닛의 갯수
* 활성화 함수의 선택

하이퍼파라미터에는 정해진 값이 없으며 매 시도마다 최적의 값을 구해 적용시켜줘야한다.