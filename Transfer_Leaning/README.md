
# Transfer Learning 
- 학습된 model을 기반으로 output layer을 바꿔 학습하는 기법

## idea 
- 인간은 과거학습한 지식으로 새로운 문제를 풀수있다.

1. output layer을 보여중인 data에 대응하는 output layer로 바꾸기
```python
# ex) vgg16
net = models.vgg16(pretrained=True)
net.classifier[6] = nn.Linear(in_features=4096, out_features=10) # output layer 
```

2. 교체한 output layer을 소량의 data로 다시 학습 
> - 이때 input layer에 가까운 부분의 결합 파라미터는 학습된 값으로 변화 시키지 않는다.
>> - input layer에 가까운 층의 Parameter도 바꿔 학습된 값으로 갱신하는 방법은 Fine Tuning이라고한다.

---

## 장점
1. 적은 data set으로 학습이 가능
2. 시간 절약
3. 보통 전이학습 모델의 성능이 더 좋다.(초,중,후반 layer feture 추출이 다름)
> ex) 이미지의 경우초,중반 layer에서는 직선,곡선의 대한 추출이기때문에 다른 글자인식에도 도움이된다.

---

## Transfer learning with VGGNet & MNIST Dataset

- Goal :  vgg16 model로 Tramsfer Learning을 이용하여 MNIST Data 학습시켜 확인해보자.
> 즉, image net을 학습한 model이 MNIST의 숫자 data들을 잘 구별할수있나 확인해보자.
- code: [Transfer learning with VGGNet & MNIST Dataset](https://github.com/Ahnho/DeepLeaning_PJ/blob/main/Transfer_Leaning/Transfer_MNIST.ipynb) 
