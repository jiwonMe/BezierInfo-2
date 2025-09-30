# 단순화된 그리기

곡선을 특정 점에서 "샘플링"한 다음 그 점들을 직선으로 연결하여 그리기 과정을 단순화할 수도 있습니다. "평탄화"라고 하는 프로세스로, 곡선을 간단한 직선, "평평한" 선의 시퀀스로 줄이고 있습니다.

"X개의 세그먼트를 원한다"고 말한 다음 원하는 세그먼트 수로 끝나도록 간격을 둔 간격으로 곡선을 샘플링하여 이를 수행할 수 있습니다. 이 방법의 장점은 빠르다는 것입니다. 100개 또는 심지어 1000개의 곡선 좌표를 평가하는 대신 훨씬 적은 수를 샘플링하고도 여전히 충분히 괜찮아 보이는 곡선으로 끝날 수 있습니다. 물론 단점은 "실제 곡선"으로 작업하는 정밀도를 잃는다는 것이므로 일반적으로 평탄화된 곡선을 사용하여 진정한 교차 감지 또는 곡률 정렬을 수행할 수 없습니다.

<div class="figure">
<graphics-element title="2차 곡선 평탄화" src="./flatten.js" data-type="quadratic">
  <input type="range" min="1" max="16" step="1" value="4" class="slide-control">
</graphics-element>
<graphics-element title="3차 곡선 평탄화" src="./flatten.js" data-type="cubic">
  <input type="range" min="1" max="24" step="1" value="8" class="slide-control">
</graphics-element>
</div>

스케치를 클릭하고 상하 방향키를 사용하여 2차 및 3차 곡선 모두의 세그먼트 수를 줄여보세요. 특정 곡률의 경우 적은 수의 세그먼트가 잘 작동하지만, 더 복잡한 곡률의 경우(3차 곡선에서 시도해보세요) 곡률 변화를 제대로 포착하려면 더 높은 수가 필요하다는 것을 알 수 있습니다.

<div class="howtocode">

### 곡선 평탄화 구현 방법

방금 지정한 알고리즘을 사용하여 구현해봅시다.

```
function flattenCurve(curve, segmentCount):
  step = 1/segmentCount;
  coordinates = [curve.getXValue(0), curve.getYValue(0)]
  for(i=1; i <= segmentCount; i++):
    t = i*step;
    coordinates.push[curve.getXValue(t), curve.getYValue(t)]
  return coordinates;
```

끝났습니다. 알고리즘을 구현했습니다. 이제 결과 "곡선"을 일련의 선으로 그리는 것만 남았습니다.

```
function drawFlattenedCurve(curve, segmentCount):
  coordinates = flattenCurve(curve, segmentCount)
  coord = coordinates[0], _coord;
  for(i=1; i < coordinates.length; i++):
    _coord = coordinates[i]
    line(coord, _coord)
    coord = _coord
```

첫 번째 좌표를 참조점으로 시작한 다음 각 점과 다음 점 사이에 선을 그립니다.

</div>
