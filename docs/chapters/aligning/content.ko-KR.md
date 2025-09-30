# 곡선 정렬하기

제어점의 x, y 좌표를 변경하여 정의할 수 있는 곡선의 수는 엄청나게 많지만, 모든 곡선이 실제로 고유한 것은 아닙니다. 예를 들어 곡선을 정의하고 90도 회전하더라도 여전히 같은 곡선이며, 다른 그리기 좌표에서지만 같은 지점에서 극값을 찾을 수 있습니다. 따라서 "고유한" 곡선으로 작업하고 있는지 확인하는 한 가지 방법은 곡선을 "축 정렬"하는 것입니다.

정렬은 곡선의 함수도 단순화합니다. 곡선을 이동(translate)하여 첫 번째 점이 (0,0)에 놓이도록 하면 *n*항 다항식 함수가 *n-1*항 함수로 바뀝니다. 차수는 같지만 항이 줄어듭니다. 그런 다음 마지막 점도 항상 x축에 놓이도록 곡선을 회전하여 좌표를 (...,0)으로 만들 수 있습니다. 이렇게 하면 y 성분에 대한 함수가 *n-2*항 함수로 더 단순화됩니다. 예를 들어 다음과 같은 3차 곡선이 있다면:

\[
\left \{ \begin{matrix}
  x = BLUE[120] \cdot (1-t)^3 BLUE[+ 35] \cdot 3 \cdot (1-t)^2 \cdot t BLUE[+ 220] \cdot 3 \cdot (1-t) \cdot t^2 BLUE[+ 220] \cdot t^3 \\
  y = BLUE[160] \cdot (1-t)^3 BLUE[+ 200] \cdot 3 \cdot (1-t)^2 \cdot t BLUE[+ 260] \cdot 3 \cdot (1-t) \cdot t^2 BLUE[+ 40] \cdot t^3
\end{matrix} \right.
\]

첫 번째 좌표가 (0,0)에 놓이도록 이동하여 모든 *x* 좌표를 -120만큼, 모든 *y* 좌표를 -160만큼 움직이면:

\[
\left \{ \begin{matrix}
  x = BLUE[0] \cdot (1-t)^3 BLUE[- 85] \cdot 3 \cdot (1-t)^2 \cdot t BLUE[+ 100] \cdot 3 \cdot (1-t) \cdot t^2 BLUE[+ 100] \cdot t^3 \\
  y = BLUE[0] \cdot (1-t)^3 BLUE[+ 40] \cdot 3 \cdot (1-t)^2 \cdot t BLUE[+ 100] \cdot 3 \cdot (1-t) \cdot t^2 BLUE[- 120] \cdot t^3
\end{matrix} \right.
\]

그런 다음 끝점이 x축에 놓이도록 곡선을 회전하면, 좌표는 (설명을 위해 정수로 반올림) 다음과 같이 됩니다:

\[
\left \{ \begin{matrix}
  x = BLUE[0] \cdot (1-t)^3 BLUE[- 85] \cdot 3 \cdot (1-t)^2 \cdot t BLUE[- 12] \cdot 3 \cdot (1-t) \cdot t^2 BLUE[+ 156] \cdot t^3 \\
  y = BLUE[0] \cdot (1-t)^3 BLUE[- 40] \cdot 3 \cdot (1-t)^2 \cdot t BLUE[+ 140] \cdot 3 \cdot (1-t) \cdot t^2 BLUE[+ 0] \cdot t^3
\end{matrix} \right.
\]

모든 0항을 제거하면:

\[
\left \{ \begin{array}{l}
  x = BLUE[- 85] \cdot 3 \cdot (1-t)^2 \cdot t BLUE[- 12] \cdot 3 \cdot (1-t) \cdot t^2 BLUE[+ 156] \cdot t^3 \\
  y = BLUE[- 40] \cdot 3 \cdot (1-t)^2 \cdot t BLUE[+ 140] \cdot 3 \cdot (1-t) \cdot t^2
\end{array} \right.
\]

원래 곡선 정의가 상당히 단순화된 것을 볼 수 있습니다. 다음 그래픽은 예제 곡선을 x축에 정렬한 결과를 보여주며, 3차 곡선의 경우 방금 예제 공식에서 사용한 좌표를 사용합니다:

<graphics-element title="2차 곡선 정렬하기" width="550" src="./aligning.js" data-type="quadratic"></graphics-element>

&nbsp;

<graphics-element title="3차 곡선 정렬하기" width="550" src="./aligning.js" data-type="cubic"></graphics-element>
