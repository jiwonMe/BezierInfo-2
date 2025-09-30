# 점진적 곡선 오프셋

어떤 거리 `s`에서 시작하지만 다른 거리 `e`에서 끝나는 점진적 오프셋을 하려면 어떻게 할까요? 곡선의 길이를 계산할 수 있다면(Legendre-Gauss 구적법 접근 방식을 사용하면 가능) 곡선 위의 어떤 점이든 "선을 따라" 얼마나 멀리 있는지 결정할 수도 있습니다. 이 지식으로 곡선을 오프셋하여 오프셋 곡선이 균일하게 넓지 않고 시작과 끝에서 두 가지 다른 오프셋 폭 사이에서 점진적이 되도록 할 수 있습니다.

일반 오프셋과 마찬가지로 곡선을 하위 곡선으로 자르고, 각 하위 곡선이 원래 곡선을 따라 어느 거리에서 시작하고 끝나는지, 그리고 각 제어점이 곡선의 어느 점에 매핑되는지 확인합니다. 이렇게 하면 하위 곡선의 각 관심 점에 대한 곡선을 따라 가는 거리를 얻습니다. "현재" 하위 곡선을 보기 전에 본 모든 하위 곡선의 총 길이를 `S`라고 하고(현재 하위 곡선이 첫 번째라면 `S`는 0), 원래 곡선의 전체 길이를 `L`이라고 하면 다음 점진 값을 얻습니다.

- start: `S`를 구간 (`0,L`)에서 구간 `(s,e)`로 매핑
- c1: `map(<strong>S+d1</strong>, 0,L, s,e)`, d1 = c1의 투영까지 곡선을 따라 가는 거리
- c2: `map(<strong>S+d2</strong>, 0,L, s,e)`, d2 = c2의 투영까지 곡선을 따라 가는 거리
- ...
- end: `map(<strong>S+length(subcurve)</strong>, 0,L, s,e)`

각 관련 점(시작, 끝, 그리고 제어점의 곡선 위 투영)에서 곡선의 법선을 알고 있으므로, 오프셋은 단순히 원래 점을 가져와서 각 점의 오프셋 거리만큼 법선 벡터를 따라 이동하는 문제입니다. 이렇게 하면 다음 결과를 얻습니다(시작 너비가 0, 끝 너비가 40픽셀이지만 상하 방향키로 제어할 수 있음):

<graphics-element title="2차 베지에 곡선 오프셋하기" src="./offsetting.js" data-type="quadratic">
  <input type="range" min="5" max="50" step="1" value="20" class="slide-control">
</graphics-element>

<graphics-element title="3차 베지에 곡선 오프셋하기" src="./offsetting.js" data-type="cubic">
  <input type="range" min="5" max="50" step="1" value="20" class="slide-control">
</graphics-element>
