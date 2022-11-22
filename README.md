
# Object Detection Project

## 📅 **프로젝트 기간**

22.08.19 ~ 22.08.31

## 📔 **프로젝트 목적**

서울시 노후 건축물의 증가가 예상되어 이를 객체인식을 이용해 이상 탐지 자동화

## 💪 **역할**

<table>
    <thead>
        <tr>
            <th>이름</th>
            <th>역할</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>국승용</td>
            <td>PPT</td>
        </tr>
         <tr>
            <td>강효진</td>
            <td>PPT</td>
        </tr>
        <tr>
            <td>김규인</td>
            <td>PPT</td>
        </tr>
        <tr>
            <td>여민희</td>
            <td>PPT</td>
        </tr>
        <tr>
            <td>이재혁</td>
            <td>PPT</td>
        </tr>
    </tbody>
</table>


## 🗄️ **데이터셋**

- AI Hub - 서울시 노후 주택 균열 데이터

- 5개의 카테고리와 데이터 20,663개 사용

    | 카테고리 | 균열 (Crack) | 박리/박락 (Peel-off) | 철근노출 (Rebar-Exposure) | 대지 (Block) | 마감 (Finish) |
    | --- | --- | --- | --- | --- | --- |
    | 계 | 4384 | 4596 | 4414 | 3559 | 3710 |


## ⚙️ **Preprocess**

- resize & padding

    - 1080 * 1440 → 416 * 416
    
    - 사진의 크기도 컸고, 일반적인 학습사이즈인 416 * 416으로 resize
    
- polygon to bounding box

    - 라벨 데이터가 polygon 형태로 되어 있어서(xyxy 형태) 이를 bounding box형태로 바꿔줌(xywh형태)
    
   ![image](https://user-images.githubusercontent.com/73925429/203080560-002f164a-affd-4b7d-99c2-7c008232c6cf.png)


## 📝 **Modeling**

- Baseline - YOLOv4
    | Params | max_batch | # of labels | subdivision | IoU_threshold | IoU_loss | mAP |
    | --- | --- | --- | --- | --- | --- | --- |
    | 계 | 10000 | 5 | 16 | 0.213 | CIoU | 77.46% |
    

- subdivision(한번에 데이터를 처리하는 양) : 16 → 32 로 증가

      weight의 변화가 적어 성능이 떨어질 것이라 예측
      
      mAP 77.46 → 74.03% 하락

- IoU_loss : CIoU → DIoU, GIoU

      큰 차이 X

- max_batch 10,000 : mAP 77.46%

      10,000 → 5,000 (감소) : underfit  mAP 68.06%
    
      10,000 → 15,000 (증가) : 성능 개선 mAP 82.03%

- IoU threshold(예측값과 실제값이 겹치는 정도의 기준)

      max_batch 10,000 → 15,000 + IoU threshold(0.213) : mAP 82.03%
      
      max_batch 10,000 → 15,000 + IoU threshold(0.750) : mAP 79.51%
    
      max_batch 10,000 → 15,000 + IoU threshold(0.950) : mAP 70.61%

- IoU normalizer(0.07 → 0.5) + learning rate 증가

      learning rate의 폭을 크게 잡았더니 결과 값이 좋지 않아 수정
      
    ![image](https://user-images.githubusercontent.com/73925429/203082171-423c9693-8474-46a9-a505-1e59e170cb5a.png)

      3번째 경우가 underfit이라고 판단해서 max_batch를 15,000으로 증가
      
      mAP 78.36% → mAP 81.78% 증가
      
- max_batch 10,000 → 15,000 + ignore_threshold(0.7 → 0.8)

      mAP 77.46%(Baseline)  → mAP 83.41%로 증가
      
- 육안으로 구분하기 힘든 라벨을 제거하면, 나머지 라벨을 헷갈리지 않고 잘 예측할거라 생각

      block 라벨 제거 → class = 4개
      
    <img width="911" alt="image" src="https://user-images.githubusercontent.com/73925429/203097133-fb0c5e58-5d06-4db1-aff4-6bb9123e19b8.png">

      성능 하락

## 📊 **수행 결과**

- Best Params

  <img width="850" alt="image" src="https://user-images.githubusercontent.com/114709620/203184699-d165aef7-29c3-4a0e-9940-f9501d5d14de.png">
  
  
-  평가지표 
    - mAP(mean Average Precision) : 여러 object detector의 Average Precision의 면적으로 평가하는 방식. AP는 인식기가 검출한 정보 중에서 ground truth와 일치하는 비율들의 평균.
    - IoU(Intersection over Union) : 실제 객체 위치 ground truth와 예측값 prediction 두 box가 중복되는 영역의 크기를 통해 평가하는 방식으로 겹치는 영역이 넓을수록 잘 예측한 것으로 평가 
  
- confidence

  ![image](https://user-images.githubusercontent.com/73925429/203095108-0aead6ed-acc2-4d1c-8e8e-9ef192993481.png)


## 🖼️ **실생활 적용**

- 성수동

  ![image](https://user-images.githubusercontent.com/73925429/203095307-4cebec18-2ffe-409e-acf8-949dfae194f5.png)

- 알파코

  ![image](https://user-images.githubusercontent.com/73925429/203095383-1507e609-7702-42e0-99a9-23e0e1884121.png)


## 🙌🏻 **보완점**

   ~추후 추가 예정~

## Reference

[1] [YOLOv4: Optimal Speed and Accuracy of Object Detection](https://arxiv.org/pdf/2004.10934.pdf): Bochkovskiy, Alexey, Chien-Yao Wang, and Hong-Yuan Mark Liao. "Yolov4: Optimal speed and accuracy of object detection." arXiv preprint arXiv:2004.10934 (2020).
