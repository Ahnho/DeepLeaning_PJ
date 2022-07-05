# SSD(Single Shot MultiBox Detector)

## 물체감지
: 한 장의 사진에 포함된 여러 물체에 대해 영역과 이름을 확인하는 작업
> :: 화상의 어디에 무엇이 비치는지 알 수 있다.

- Bounding box(BBox): 물체의 위치를 나타내는 테두리 

- input : 화상, output : 정보 (BBox의 위치와 크기, 라벨 , 검색신뢰도 = confidence)
> - 바운딩 박스의 정보 : 화상의 왼쪽 상단을 원점(0,0)으로 두고 박스의 중심 좌표를 (cs,cy) 높이와 너비를  h,w로 표시한다.
> - 라벨정보 : 감지하려는 물체의 클래스 수 O 에 background를 더한 (O+1)종류의 클래스로 각 바운딩 박스당 하나의 라베를 구한다.
> - confidence : 각 바운딩 박스와 라벨의 신뢰도를 보여줌, 신뢰도가 높은 바운딩 박스 하나만을 최종 출력한다.

---

- VOC2012 dataset 사용 : class : 20 , training dataset:5717, validation dataset :5823
> - background를 포함하여 총 21개의 class 사용 
> - VOC data는 사진 좌측상단이 (0,0)이 아닌 (1,1)이다.

## SSD의 물체감지 흐름(SSD_300)
- SSD는 입력화상을 300x300 or 512x512 pixel로 처리하는 SSD 300,SSD 512 두가지 패턴이 있다. 
- BBox를 도출할때 BBox의 정보를 출력 하는 것이 아니라, DBox(default box)를 준비해두고 어떻게 변형시키면 BBox가 되는지에 대한 정보를 출력한다.
>::  offset 정보 : DBox를 변형시키는 정보
- DBOX의정보$(cx_d,cy_d,w_d,h_d)$ 인 경우 offset 정보는$(\Delta cx_d,\Delta cy_d,\Delta w_d,\Delta h_d)$의 4변수 
> - $cx = cx_d + 0.1\Delta cx \times w_d$
> - $cy = cy_d + 0.1\Delta cy \times h_d$
> - $w = w_d \times exp(0.2 \Delta w)$
> - $h = h_d \times exp(0.2 \Delta h)$
> - :: 위 계산식은 이론적으로 도출된 식이 아닌, SSD에서 규정하고 딥러닝 모델을 학습시켜 만들어신 식이다.

- step
> 1. 전처리로 $300 \times 300$ pixel로 resize한다... 이때 색 정보의 표준화도 실행
> 2. 다양한 크기 DBox를 준비, SSD_300의 경우 8732개의 DBOX 준비(모든 화상에 대해 동등하게 준비)
> 3. 전처리한 화상을 SSD_net에 입력, offset 4개 클래스 신뢰도 21개의 합계 8732 x (4+21) = 218,300개의 정보 출력
> 4. 8732개의 DBox의 중 condidence가 높은 상위 top_k개를 추출(SSD 300에서는 200개 추출)
> 5. offset 정보를통해 DBox -> BBox로 변형 step_4에서 꺼낸 top_k개의 DBox중 BBox와 겹치는 것이 많으면 그중 confidence가 높은 BBox만 남긴다.
> 6. 최종적으로 BBox와 라벨을 출력한다.
>> :: confidence의 임계치를 결정해 그 이상의 confidence를 가장 BBox만 출력 
>> :: 이때, 잘못된 검출을 피하고 싶으면 임계값을 높이 설정 

---

## References
- 만들면서 배우는 파이토치 딥러닝, 오가와 유타로 저, 박광수 옮김, 한빛미디어 (2021)