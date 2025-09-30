# 베지에 곡선의 곡률 제어하기

베지에 곡선은 모든 "스플라인"처럼 보간 함수입니다. 즉, 여러 개의 점을 받아서 그 점들 "사이" 어딘가의 값을 생성한다는 뜻이죠. (그래서 제어점들을 연결한 외곽선, 일반적으로 곡선의 "포(hull)"이라고 부르는 영역 밖에 있는 점은 절대 생성할 수 없습니다. 유용한 정보죠!) 실제로 각 점이 함수가 생성하는 값에 얼마나 기여하는지 시각화할 수 있어서, 곡선의 어느 위치에서 어떤 점이 중요한지 볼 수 있습니다.

아래 그래프는 2차 및 3차 곡선의 보간 함수를 보여줍니다. "S"는 베지에 함수 전체 값에 대한 각 점의 기여도입니다. 클릭해서 드래그하면 특정 <i>t</i> 값에서 곡선을 정의하는 각 점의 보간 비율을 확인할 수 있습니다.

<div class="figure">
<graphics-element title="2차 보간" src="./lerp.js" data-degree="3">
  <input type="range" min="0" max="1" step="0.01" value="0" class="slide-control">
</graphics-element>
<graphics-element title="3차 보간" src="./lerp.js" data-degree="4">
  <input type="range" min="0" max="1" step="0.01" value="0" class="slide-control">
</graphics-element>
<graphics-element title="15차 보간" src="./lerp.js" data-degree="15">
  <input type="range" min="0" max="1" step="0.01" value="0" class="slide-control">
</graphics-element>
</div>

15차 베지에 함수의 보간 함수도 함께 보여드립니다. 보시다시피 시작점과 끝점이 제어점 집합의 다른 어떤 점보다 곡선의 형태에 훨씬 더 많이 기여합니다.

곡선을 변경하려면 각 점의 가중치를 바꿔서 보간을 효과적으로 변경해야 합니다. 방법은 아주 간단합니다. 각 점에 그 점의 영향력을 변경하는 값을 곱하면 됩니다. 이 값들을 관례적으로 "가중치(weights)"라고 부르며, 원래 베지에 함수에 추가할 수 있습니다.

\[
  Bézier(n,t) = \sum_{i=0}^{n}
                \underset{\textit{이항 항}}{\underbrace{\binom{n}{i}}}
                \cdot\
                \underset{\textit{다항식 항}}{\underbrace{(1-t)^{n-i} \cdot t^{i}}}
                \cdot\
                \underset{\textit{가중치}}{\underbrace{w_i}}
\]

복잡해 보이지만, 사실 "가중치"는 그냥 곡선이 가져야 할 좌표 값입니다. <i>n</i>차 곡선의 경우 w<sub>0</sub>은 시작 좌표, w<sub>n</sub>은 마지막 좌표, 그 사이의 모든 것은 제어 좌표입니다. 예를 들어 (110,150)에서 시작해서 (25,190)과 (210,250)로 제어되고 (210,30)에서 끝나는 3차 곡선을 만들고 싶다면 이런 베지에 곡선을 사용합니다.

\[
\left \{ \begin{matrix}
  x = DARKRED[110] \cdot (1-t)^3 + DARKGREEN[25] \cdot 3 \cdot (1-t)^2 \cdot t + DARKBLUE[210] \cdot 3 \cdot (1-t) \cdot t^2 + AMBER[210] \cdot t^3 \\
  y = DARKRED[150] \cdot (1-t)^3 + DARKGREEN[190] \cdot 3 \cdot (1-t)^2 \cdot t + DARKBLUE[250] \cdot 3 \cdot (1-t) \cdot t^2 + AMBER[30] \cdot t^3
\end{matrix} \right.
\]

이렇게 하면 글 맨 위에서 본 곡선이 나옵니다.

<graphics-element title="3차 베지에 곡선" src="../introduction/cubic.js"></graphics-element>

베지에 곡선으로 또 뭘 할 수 있을까요? 사실 정말 많습니다. 이 글의 나머지 부분에서는 적용할 수 있는 다양한 연산과 알고리즘, 그리고 그것들로 해결할 수 있는 작업들을 다룹니다.

<div class="howtocode">

### 가중치 기저 함수 구현 방법

이미 기저 함수를 구현하는 방법을 알고 있으니, 제어점을 추가하는 것은 놀랍도록 쉽습니다.

```
function Bezier(n,t,w[]):
  sum = 0
  for(k=0; k<=n; k++):
    sum += w[k] * binomial(n,k) * (1-t)^(n-k) * t^(k)
  return sum
```

그리고 최적화된 버전들은 다음과 같습니다.

```
function Bezier(2,t,w[]):
  t2 = t * t
  mt = 1-t
  mt2 = mt * mt
  return w[0]*mt2 + w[1]*2*mt*t + w[2]*t2

function Bezier(3,t,w[]):
  t2 = t * t
  t3 = t2 * t
  mt = 1-t
  mt2 = mt * mt
  mt3 = mt2 * mt
  return w[0]*mt3 + 3*w[1]*mt2*t + 3*w[2]*mt*t2 + w[3]*t3
```

이제 가중치 기저 함수를 프로그래밍하는 방법을 알게 되었습니다.

</div>
