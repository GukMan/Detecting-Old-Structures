
# Object Detection Project

---

## 📅 **프로젝트 기간**

22.08.19 ~ 22.08.31

---

## 📔 **프로젝트 목적**

서울시 노후 건축물의 증가가 예상되어 이를 객체인식을 이용해 이상 탐지 자동화

---

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

---

## 🗄️ **데이터셋**

- AI Hub - 서울시 노후 주택 균열 데이터
- 5개의 카테고리와 데이터 20,663개 사용
    | 카테고리 | 균열 (Crack) | 박리/박락 (Peel-off) | 철근노출 (Rebar-Exposure) | 대지 (Block) | 마감 (Finish) |
    | --- | --- | --- | --- | --- | --- |
    | 계 | 4384 | 4596 | 4414 | 3559 | 3710 |

---

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
