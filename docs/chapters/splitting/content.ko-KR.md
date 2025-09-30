# 곡선 분할하기

드 카스텔죠 알고리즘을 사용하면 베지에 곡선을 두 개의 더 작은 곡선으로 분할하는 데 필요한 모든 점을 찾을 수도 있습니다. 이 두 곡선을 합치면 원래 곡선을 형성합니다. 어떤 값 `t`에 대해 드 카스텔죠 골격을 구성하면, 이 절차는 해당 `t` 값에서 곡선을 분할하는 데 필요한 모든 점을 제공합니다. 한 곡선은 곡선 위의 점 이전에 찾은 모든 내부 골격 점으로 정의되고, 다른 곡선은 곡선 위의 점 이후의 모든 내부 골격 점으로 정의됩니다.

<graphics-element title="곡선 분할하기" width="825" src="./splitting.js">
  <input type="range" min="0" max="1" step="0.01" value="0.5" class="slide-control">
</graphics-element>

<div class="howtocode">

### 곡선 분할 구현

드 카스텔죠 함수에 추가 로깅을 붙여서 곡선 분할을 구현할 수 있습니다.

```
left=[]
right=[]
function drawCurvePoint(points[], t):
  if(points.length==1):
    left.add(points[0])
    right.add(points[0])
    draw(points[0])
  else:
    newpoints=array(points.size-1)
    for(i=0; i<newpoints.length; i++):
      if(i==0):
        left.add(points[i])
      if(i==newpoints.length-1):
        right.add(points[i+1])
      newpoints[i] = (1-t) * points[i] + t * points[i+1]
    drawCurvePoint(newpoints, t)
```

어떤 값 `t`에 대해 이 함수를 실행한 후, `left`와 `right` 배열에는 두 개의 새 곡선에 대한 모든 좌표가 포함됩니다. 하나는 `t` 값의 "왼쪽"에, 다른 하나는 "오른쪽"에 있습니다. 이 새 곡선들은 원래 곡선과 같은 차수를 가지며 원래 곡선 위에 정확히 겹쳐질 수 있습니다.

</div>
