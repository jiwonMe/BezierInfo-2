# 드 카스텔죠 알고리즘

베지에 곡선을 그리려면 `t` 값을 0부터 1까지 순회하면서 각 값에서 가중치 기저 함수를 계산해 그려야 할 `x/y` 값을 얻으면 됩니다. 하지만 곡선이 복잡해질수록 이 계산도 비용이 많이 듭니다. 대신 *드 카스텔죠 알고리즘*을 사용해서 곡선을 그릴 수 있습니다. 이것은 기하학적으로 곡선을 그리는 방법이고, 구현도 정말 쉽습니다. 너무 쉬워서 연필과 자만 있으면 손으로도 할 수 있을 정도예요.

`t`에 대한 `x/y` 값을 찾기 위해 미적분 함수를 사용하는 대신, 이렇게 해봅시다.

- `t`를 비율로 취급합니다(실제로 그렇죠). t=0은 선의 0% 지점, t=1은 선의 100% 지점입니다.
- 곡선을 정의하는 점들 사이에 모든 선을 그립니다. `n`차 곡선이라면 `n`개의 선이 있습니다.
- 각 선을 따라 `t` 거리에 표시를 합니다. 그러니까 `t`가 0.2라면 시작점에서 20%, 끝점에서 80% 지점에 표시합니다.
- 이제 `그` 점들 사이에 선을 만듭니다. `n-1`개의 선이 나옵니다.
- 이 선들을 따라 `t` 거리에 표시를 합니다.
- `그` 점들 사이에 선을 만듭니다. `n-2`개의 선이 됩니다.
- 표시하고, 선 그리고, 표시하고, 등등.
- 선이 하나만 남을 때까지 반복합니다. 그 선의 `t` 위치에 있는 점이 원래 곡선의 `t` 지점과 일치합니다.

실제로 보려면 아래 스케치의 슬라이더를 움직여서 드 카스텔죠 알고리즘을 사용해 명시적으로 계산되는 곡선 점을 변경해보세요.

<graphics-element title="드 카스텔죠 알고리즘으로 곡선 따라가기" src="./decasteljau.js">
  <input type="range" min="0" max="1" step="0.01" value="0" class="slide-control">
</graphics-element>

<div class="howtocode">

### 드 카스텔죠 알고리즘 구현 방법

방금 설명한 알고리즘을 그대로 사용하고, 곡선을 정의하는 점들의 리스트와 `t` 값을 받아서 그 `t` 값에 대응하는 곡선 위의 점을 그리는 함수로 구현해봅시다.

```
function drawCurvePoint(points[], t):
  if(points.length==1):
    draw(points[0])
  else:
    newpoints=array(points.size-1)
    for(i=0; i<newpoints.length; i++):
      newpoints[i] = (1-t) * points[i] + t * points[i+1]
    drawCurvePoint(newpoints, t)
```

끝입니다. 알고리즘을 구현했네요. 하지만 보통은 "+" 연산자를 오버로딩할 수 있는 사치를 누릴 수 없으니, `x`와 `y` 값을 따로 다뤄야 할 때의 코드도 제공하겠습니다.

```
function drawCurvePoint(points[], t):
  if(points.length==1):
    draw(points[0])
  else:
    newpoints=array(points.size-1)
    for(i=0; i<newpoints.length; i++):
      x = (1-t) * points[i].x + t * points[i+1].x
      y = (1-t) * points[i].y + t * points[i+1].y
      newpoints[i] = new point(x,y)
    drawCurvePoint(newpoints, t)
```

그래서 이게 뭘 하는 걸까요? 전달된 점 리스트의 길이가 1이면 점을 그립니다. 그게 아니면 <i>t</i> 비율에 위치하는 새로운 점들의 리스트를 만들고(즉, 위 알고리즘에서 설명한 "표시"), 이 새 리스트로 그리기 함수를 호출합니다.

</div>
