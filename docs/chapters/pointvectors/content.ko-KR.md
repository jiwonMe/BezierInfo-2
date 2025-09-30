# 접선과 법선

곡선을 따라 객체를 이동하거나 곡선에서 "멀어지게" 하려면, 가장 관심 있는 두 벡터는 곡선 점에 대한 접선 벡터와 법선 벡터입니다. 이것들은 실제로 찾기 정말 쉽습니다. 곡선을 따라 이동하고 방향을 잡기 위해 접선을 사용하는데, 이것은 특정 점에서의 이동 방향을 나타내며, 문자 그대로 곡선의 1차 도함수일 뿐입니다.

\[
\begin{matrix}
  \textit{tangent}_x(t) = B'_x(t) \\
  \\
  \textit{tangent}_y(t) = B'_y(t)
\end{matrix}
\]

이것은 우리가 원하는 방향 벡터를 제공합니다. 이를 정규화하여 각 점에서 균일한 방향 벡터(길이가 1.0인)를 얻은 다음 그 방향을 기반으로 하려는 작업을 수행할 수 있습니다.

\[
\begin{matrix}
  d = \left \| \textit{tangent}(t) \right \| = \sqrt{B'_x(t)^2 + B'_y(t)^2} \\
  \\
  \hat{x}(t) = \left \| \textit{tangent}_x(t) \right \|
             =\frac{\textit{tangent}_x(t)}{ \left \| \textit{tangent}(t) \right \| }
             = \frac{B'_x(t)}{d} \\
  \\
  \hat{y}(t) = \left \| \textit{tangent}_y(t) \right \|
             = \frac{\textit{tangent}_y(t)}{ \left \| \textit{tangent}(t) \right \| }
             = \frac{B'_y(t)}{d}
\end{matrix}
\]

접선은 선을 따라 이동하는 데 매우 유용하지만, 대신 어떤 점 <i>t</i>에서 곡선에 수직으로 곡선에서 멀어지고 싶다면 어떻게 할까요? 그 경우 *법선* 벡터를 원합니다. 이 벡터는 곡선 방향에 직각으로 실행되며 일반적으로 길이가 1.0이므로, 정규화된 방향 벡터를 회전하기만 하면 끝입니다.

\[
\begin{array}{l}
  \textit{normal}_x(t) = \hat{x}(t) \cdot \cos{\frac{\pi}{2}} - \hat{y}(t) \cdot \sin{\frac{\pi}{2}} = - \hat{y}(t) \\
  \\
  \textit{normal}_y(t) = \underset{\textit{quarter circle rotation}} {\underbrace{ \hat{x}(t) \cdot \sin{\frac{\pi}{2}} + \hat{y}(t) \cdot \cos{\frac{\pi}{2}} }} = \hat{x}(t)
\end{array}
\]

<div class="note">

좌표를 회전하는 것은 실제로 규칙을 알면 매우 쉽습니다. "[회전 행렬](https://en.wikipedia.org/wiki/Rotation_matrix) 적용"으로 설명된 것을 볼 수 있는데, 여기서도 살펴보겠습니다. 본질적으로 아이디어는 회전할 수 있는 원을 가져와서 원하는 각도만큼 "좌표를 원 위로 미끄러뜨리는" 것입니다. 4분원 회전을 원하면 좌표를 가져와서 원을 따라 4분의 1 회전만큼 미끄러뜨리면 끝입니다.

어떤 점 <i>(x,y)</i>를 (0,0 위로) 어떤 각도 φ만큼 회전된 점 <i>(x',y')</i>로 바꾸려면 이 멋지고 쉬운 계산을 적용합니다.

\[\begin{array}{l}
  x' = x \cdot \cos(\phi) - y \cdot \sin(\phi) \\
  y' = x \cdot \sin(\phi) + y \cdot \cos(\phi)
\end{array}\]

이것은 다음 행렬 변환의 "긴" 버전입니다.

\[
  \begin{bmatrix}
    x' \\ y'
  \end{bmatrix}
  =
  \begin{bmatrix}
   \cos(\phi) & -\sin(\phi) \\
   \sin(\phi) & \cos(\phi)
  \end{bmatrix}
  \begin{bmatrix}
    x \\ y
  \end{bmatrix}
\]

그리고 그것이 좌표를 회전하는 데 필요한 전부입니다. 4분원, 반 회전, 3/4 회전의 경우 이러한 함수는 훨씬 더 쉬워집니다. 이러한 각도에 대한 *sin*과 *cos*는 각각: 0과 1, -1과 0, 0과 -1이기 때문입니다.

그런데 ***왜*** 이것이 작동할까요? 왜 이 행렬 곱셈일까요? [Wikipedia](https://en.wikipedia.org/wiki/Rotation_matrix#Decomposition_into_shears) (기술적으로는 Thomas Herter와 Klaus Lott)는 회전 행렬을 세 개의 (기본) 전단 연산 시퀀스로 처리할 수 있다고 알려줍니다. 이것을 단일 행렬 연산으로 결합하면(모든 행렬 곱셈은 축소될 수 있으므로) 위에서 본 행렬을 얻습니다. [DataGenetics](https://datagenetics.com/blog/august32013/index.html)에는 바로 이것에 대한 훌륭한 기사가 있습니다. 정말 멋지며, 이 입문서에서 잠깐 휴식을 취하고 그 기사를 읽는 것을 강력히 추천합니다.

</div>

다음 두 그래픽은 2차 및 3차 곡선을 따라 접선과 법선을 보여주며, 방향 벡터는 파란색으로, 법선 벡터는 빨간색으로 표시됩니다(마커는 등거리가 아닌 *t* 간격으로 균등하게 간격이 띄워져 있습니다).

<div class="figure">
  <graphics-element title="2차 베지에 접선과 법선" src="./pointvectors.js" data-type="quadratic"></graphics-element>
  <graphics-element title="3차 베지에 접선과 법선" src="./pointvectors.js" data-type="cubic"></graphics-element>
</div>
