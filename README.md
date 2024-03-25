# Make-Cartoon
Cartoonize real world

### 1. 기능요약
   주어진 이미지를 카툰(Cartoon) 스타일로 변환한다

### 2. 프로그램 설명

```python
import cv2
import numpy as np
```
numpy와 opencv 기능을 불러온다

```python
def cartoonize(image):
    # 이미지를 블러처리하여 경계를 부드럽게 만든다
    blurred = cv2.bilateralFilter(image, 9, 75, 75)

    # 그레이스케일로 변환한다
    gray = cv2.cvtColor(blurred, cv2.COLOR_BGR2GRAY)

    # 엣지 검출을 수행한다
    edges = cv2.adaptiveThreshold(gray, 255, cv2.ADAPTIVE_THRESH_MEAN_C, cv2.THRESH_BINARY, 9, 9)

    # 엣지 강조를 위해 컬러 이미지로 변환한다
    color = cv2.edgePreservingFilter(image, flags=2, sigma_s=50, sigma_r=0.4)

    # 엣지 영역과 컬러 이미지를 합성하여 만화 스타일을 만든다
    cartoon = cv2.bitwise_and(color, color, mask=edges)

    return cartoon
```
카툰 스타일로 변환하기 위한 함수

```python
image_path = 'test.jpg'
image = cv2.imread(image_path)
```
이미지를 불러온다

```python
cartoon_image = cartoonize(image)
```
카툰 스타일로 변환된 이미지를 얻는다

```python
# 결과 이미지 출력
cv2.imshow('Cartoon Image', cartoon_image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
결과 이미지를 출력한다

### 3. 실제 실행 이미지 비교
변환이 잘 이루어지지 않은 케이스
<p align="center" width="100%">
  <img src="https://github.com/b0v0d/Make-Cartoon/assets/162780235/4cbea48a-c91d-4c32-b0bf-582f97a2bf2b" width="49%">
  <img src="https://github.com/b0v0d/Make-Cartoon/assets/162780235/2b638fc0-e83f-45dd-a0dd-46d0705a5807" width="49%">
</p>
변환이 잘 이루어진 케이스
<p align="center" width="100%">
  <img src="https://github.com/b0v0d/Make-Cartoon/assets/162780235/d92b598d-ecd1-4f3a-9d22-5c26f10fcaa3" width="49%">
  <img src="https://github.com/b0v0d/Make-Cartoon/assets/162780235/03850eca-fb33-4523-b67a-1af0641ede7c" width="49%">
</p>

### 4. 코드의 한계 분석
변환이 이루어진 케이스도 사실 완벽한 변환이 된 것은 아니다. 손의 오른 쪽 부분을 보면, 빛을 받은 손이 배경의 색과 거의 흡사하여 경계로 인식되지 않았다.
이는 그림의 전체 문맥을 보고 종합적으로 판단하는 사람과 달리, 단순히 인접한 영역의 색차이를 보고 경계를 판단하는 알고리즘의 한계라고 생각한다. 변환이
잘 이루어지지 않은 경우 맨 앞에 있는 나무는 선명하여 결과가 뚜렷한 반면, 상대적으로 뒤에 있는 나무들은 흐리기 때문에 openCV의 알고리즘이 인식을 하지 
못한 것이라 판단된다.
