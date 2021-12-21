### 20101202 컴퓨터공학과 김동원

### 주제 : 파이썬으로 노노그램 만들기

* 노노그램이란?

X×Y 크기의 직사각형에 각각 적혀있는 숫자를 보고 숨어있는 숫자를 예측해서 지우고 그려나가면서 그림을 만들어가는 게임.

* 노노그램을 어떻게 만들것인가?

특정 색깔로 이미 색칠되어 표현된 노노그램을 준비한다. 색칠되어있는 칸을 레이블링 하여 배열에 값을 저장 후 가로와 세로로 구분하여 노노그램을 표현한다.

{ 예시 }
<img width="80%" src="https://user-images.githubusercontent.com/77396001/146914360-f3ae70d9-d8f7-463e-87f1-08294ec2f88d.png"/>

* 코드와 부분별 결과

1. 노노그램 이미지 불러오기.

-------------Python Code-------------
=====================================

```python
import numpy as np
import cv2, torch
from google.colab.patches import cv2_imshow

!wget -c 'https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FATOKW%2Fbtrorl81wQK%2FUH74EY52a8G75CmMr8bOYk%2Fimg.png' -O 'nonogram.jpg'

nonogram = cv2.imread('nonogram.jpg')
cv2_imshow(nonogram)

# 수업 시간에 배웠던 코드를 이용하여 웹페이지에 있는 색칠되어있는 노노그램 이미지를 다운받는다.
# => 해당 이미지는 http://www.landofcrispy.com/nonogrammer/nonogram.html?mode=build 사이트에서 제작했으며
# https://doxdef.tistory.com/7 작성자 본인의 블로그에 올려서 해당 이미지 주소를 참조하였다.

```

결과

<img width="100%" src="https://user-images.githubusercontent.com/77396001/146914428-e665da23-a995-4a6d-afe4-b25bce8124d5.PNG"/>

=====================================

2. 노노그램 레이블링

-------------Python Code-------------
=====================================

```python
img_color = cv2.imread('nonogram.jpg') 
height, width = img_color.shape[:2]
img_hsv = cv2.cvtColor(img_color, cv2.COLOR_BGR2HSV) 
lower_red = (0, 255, 255) 
upper_red = (0, 255, 255) 
img_mask = cv2.inRange(img_hsv, lower_red, upper_red) 
img_result = cv2.bitwise_and(img_color, img_color, mask = img_mask)

cnt, labels, stats, centroids = cv2.connectedComponentsWithStats(img_mask)

dst = cv2.cvtColor(img_mask, cv2.COLOR_GRAY2BGR)

for i in range(1, cnt):
    (x, y, w, h, area) = stats[i]

    cv2.putText(dst, str(i), (x + w // 2 - 5, y + h // 2 + 5), cv2.FONT_HERSHEY_PLAIN, 1, (0, 0, 255), 1, cv2.LINE_AA)
    cv2.rectangle(dst, (x, y, w, h), (0, 0, 255))

cv2_imshow(img_color) 
cv2_imshow(img_mask) 
cv2_imshow(img_result) 
cv2_imshow(dst)
cv2.waitKey(0) 
cv2.destroyAllWindows()

# 빨간색으로 색칠되어 있는 칸을 레이블링하여 총 몇 개의 칸이 색칠되었는지 구분한다.
# 각각의 칸의 정보는 stats[i]에 저장된다.
# 해당 코드는 https://mangchhe.github.io/imageprocess/2021/07/16/LabelingFromImage/ 와
# https://velog.io/@nayeon_p00/OpenCV-Python-HSV-%EC%83%89%EA%B3%B5%EA%B0%84%EC%97%90%EC%84%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80%EC%9D%98-%ED%8A%B9%EC%A0%95%EC%83%89-%EC%B6%94%EC%B6%9C%ED%95%98%EA%B8%B0
# 의 코드를 참고하여 작성되었다.

```

결과

<img width="40%" src="https://user-images.githubusercontent.com/77396001/146915277-0eb3f68f-8857-45a6-b256-07b339772dd0.png"/>

=====================================

3. 노노그램 작성

-------------Python Code-------------
=====================================

```python
num = 1
print("row")
count = 1
row = 1
for i in range(1, (cnt - 1)):
  if (stats[i][1] != row):
    print(0)
    row += 31
  if(stats[i][1] == stats[i+1][1]):
      if (stats[i][0] + 33 == stats[i+1][0]):
        count+=1
      else:
        print(count, end = ' ')
        count = 1
  else:
   print(count)
   count = 1
   num+=1
   if (stats[i][1] == row):
    row += 31
print(count)
if (num < 10):
  print(0)
print("=========")
print("column")
num = 1
count = 1
column = 2
stats = stats[stats[:, 1].argsort(kind='mergesort')]
stats = stats[stats[:, 0].argsort(kind='mergesort')]  
for i in range(1, (cnt - 1)):
  if (stats[i][0] != column):
    print(0)
    num+=1
    column += 33
  if(stats[i][0] == stats[i+1][0]):
      if (stats[i][1] + 31 == stats[i+1][1]):
        count+=1
      else:
        print(count, end = ' ')
        count = 1
  else:
   print(count)
   count = 1
   num+=1
   if (stats[i][0] == column):
    column += 33
print(count)
if (num < 10):
  print(0)  
  
```
stats[i]에 저장되어있는 결과는 다음과 같다.

[ 68   1  32  30 960]<br/>
[167   1  32  30 960]<br/>
[233   1  32  30 960]<br/>
[ 35  32  32  30 960]<br/>
[ 68  32  32  30 960]<br/>
[167  32  32  30 960]<br/>
[200  32  32  30 960]<br/>
[ 68  63  32  30 960]<br/>
[167  63  32  30 960]<br/>
[ 68  94  32  30 960]<br/>
[101  94  32  30 960]<br/>
[134  94  32  30 960]<br/>
[167  94  32  30 960]<br/>
[200  94  32  30 960]<br/>
[233  94  32  30 960]<br/>
[ 68 125  32  30 960]<br/>
[200 125  32  30 960]<br/>
[233 125  32  30 960]<br/>
[ 35 156  32  30 960]<br/>
[ 68 156  32  30 960]<br/>
[233 156  32  30 960]<br/>
[ 35 187  32  30 960]<br/>
[134 187  32  30 960]<br/>
[167 187  32  30 960]<br/>
[233 187  32  30 960]<br/>
[266 187  32  30 960]<br/>
[ 35 218  32  30 960]<br/>
[134 218  32  30 960]<br/>
[167 218  32  30 960]<br/>
[266 218  32  30 960]<br/>
[ 35 249  32  30 960]<br/>
[ 68 249  32  30 960]<br/>
[167 249  32  30 960]<br/>
[266 249  32  30 960]<br/>
[ 68 280  32  29 928]<br/>
[101 280  32  29 928]<br/>
[134 280  32  29 928]<br/>
[200 280  32  29 928]<br/>
[233 280  32  29 928]<br/>

이 결과를 바탕으로 row를 생성 후 
column을 생성하기 위해 정렬한다. 그 결과

[ 35  32  32  30 960]<br/>
[ 35 156  32  30 960]<br/>
[ 35 187  32  30 960]<br/>
[ 35 218  32  30 960]<br/>
[ 35 249  32  30 960]<br/>
[ 68   1  32  30 960]<br/>
[ 68  32  32  30 960]<br/>
[ 68  63  32  30 960]<br/>
[ 68  94  32  30 960]<br/>
[ 68 125  32  30 960]<br/>
[ 68 156  32  30 960]<br/>
[ 68 249  32  30 960]<br/>
[ 68 280  32  29 928]<br/>
[101  94  32  30 960]<br/>
[101 280  32  29 928]<br/>
[134  94  32  30 960]<br/>
[134 187  32  30 960]<br/>
[134 218  32  30 960]<br/>
[134 280  32  29 928]<br/>
[167   1  32  30 960]<br/>
[167  32  32  30 960]<br/>
[167  63  32  30 960]<br/>
[167  94  32  30 960]<br/>
[167 187  32  30 960]<br/>
[167 218  32  30 960]<br/>
[167 249  32  30 960]<br/>
[200  32  32  30 960]<br/>
[200  94  32  30 960]<br/>
[200 125  32  30 960]<br/>
[200 280  32  29 928]<br/>
[233   1  32  30 960]<br/>
[233  94  32  30 960]<br/>
[233 125  32  30 960]<br/>
[233 156  32  30 960]<br/>
[233 187  32  30 960]<br/>
[233 280  32  29 928]<br/>
[266 187  32  30 960]<br/>
[266 218  32  30 960]<br/>

stats[i]는 위와 같이 정렬된다.

노노그램의 예시를 하나밖에 들지는 못했지만, 이번 프로젝트에서 적용한 노노그램의 경우 row와 column에서 규칙성을 알 수 있었다.

<img width="{80%}" src="{https://user-images.githubusercontent.com/77396001/146916053-7823a1a2-fc3a-4f27-81fb-4ff0b596b365.png}"/>

row값은 초기값이 2이고 이후 33씩 증가하며, column값은 초기값이 1이고 이후 31씩 증가한다.
위와 같은 규칙을 이용하여 노노그램을 작성하는 알고리즘을 작성하였고 다음과 같은 결과가 도출되었다.

row<br/>
1 1 1<br/>
2 2<br/>
1 1<br/>
6<br/>
1 2<br/>
2 1<br/>
1 2 2<br/>
1 2 1<br/>
2 1 1<br/>
3 2<br/>
<br/>
column<br/>
0<br/>
1 4<br/>
6 2<br/>
1 1<br/>
1 2 1<br/>
4 3<br/>
1 2 1<br/>
1 4 1<br/>
3<br/>
0<br/>

원본과 비교했을 때 정확히 일치한다는 것을 알 수 있다.

<img width="60%" src="https://user-images.githubusercontent.com/77396001/146916750-9294cc75-0dc4-4e6a-b864-1639160c5f73.PNG"/>

### 전체 코드 확인하기 : https://github.com/DOXDEF/DOXDEF_Main/blob/main/NONOGRAM.ipynb

=====================================

* 결론 및 아쉬운 점

 - 1개의 예시에서 도출된 row와 column값이 다른 노노그램에도 적용된다는 점을 보장하기 어렵기 때문에 여러 개의 예시를 통해 노노그램 생성 알고리즘을 좀 더 보완해야할 필요가 느껴진다.
 - column값을 생성하는데 stats 배열을 정렬해야하는 귀찮음이 동반되었다. 가로가 아닌 세로로 레이블링이 가능한지 알아봐야겠다.
 - 일반적인 노노그램들은 주로 검은색으로 칠하기 마련이다. 검은색으로 실행시 오류가 나서 빨간색으로 칠한 노노그램에만 한정된 코드를 작성하였다. 검은색 뿐만아니라 다른 색에도 적용될 수 있는 범용적인 알고리즘을
   만들 필요성을 느꼈다.


### 참고자료/reference


* https://mangchhe.github.io/imageprocess/2021/07/16/LabelingFromImage/
* https://velog.io/@nayeon_p00/OpenCV-Python-HSV-%EC%83%89%EA%B3%B5%EA%B0%84%EC%97%90%EC%84%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80%EC%9D%98-%ED%8A%B9%EC%A0%95%EC%83%89-%EC%B6%94%EC%B6%9C%ED%95%98%EA%B8%B0

=> 레이블링 코드 참고

