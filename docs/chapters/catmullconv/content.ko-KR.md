# 베지에 곡선과 Catmull-Rom 곡선

다른 스플라인으로 소풍을 가면, 다른 일반적인 디자인 곡선은 [Catmull-Rom 스플라인](https://en.wikipedia.org/wiki/Cubic_Hermite_spline#Catmull.E2.80.93Rom_spline)입니다. 베지에 곡선과 달리 각 제어점을 _통과_하므로 일종의 "내장" 곡선 맞추기를 제공합니다.

사실 하나를 가지고 노는 것부터 시작해봅시다. 다음 그래픽에는 점을 조작할 수 있는 사전 정의된 곡선이 있으며, 배경을 클릭/탭하여 점을 추가할 수 있고, 장력 슬라이더를 사용하여 곡선이 점을 통과하는 "속도"를 제어할 수 있습니다. 곡선이 더 긴장할수록 곡선은 한 점에서 다음 점으로의 직선으로 더 향하는 경향이 있습니다.

<graphics-element title="Catmull-Rom 곡선" src="./catmull-rom.js">
  <input type="range" min="0.1" max="1" step="0.01" value="0.5" class="slide-control tension">
</graphics-element>

이제 Catmull-Rom 곡선이 베지에 곡선과 매우 다른 것처럼 보일 수 있습니다. 이러한 곡선이 실제로 매우 길어질 수 있기 때문입니다. 하지만 단일 Catmull-Rom 곡선처럼 보이는 것은 실제로 [스플라인](https://en.wikipedia.org/wiki/Spline_(mathematics))입니다. 많은 동일하게 계산된 조각으로 구성된 단일 곡선으로, 많은 베지에 곡선을 가져와서 끝에서 끝까지 배치하고 제어점을 정렬하여 단일 곡선처럼 보이도록 하는 것과 유사합니다. Catmull-Rom 곡선의 경우 두 점 사이의 각 "조각"은 점의 좌표와 해당 점의 접선으로 정의되며, 후자는 이전 점과 다음 점을 알면 [사소하게 도출될 수 있습니다](https://en.wikipedia.org/wiki/Cubic_Hermite_spline#Catmull%E2%80%93Rom_spline).

\[
  \begin{bmatrix}
    P_1 \\
    P_2 \\
    P_3 \\
    P_4
  \end{bmatrix}_{points}
  =
  \left [
    \begin{array}{rl}
      V_1 &= P_2 \\
      V_2 &= P_3 \\
      V'_1 &= \frac{P_3 - P_1}{2} \\
      V'_2 &= \frac{P_4 - P_2}{2}
    \end{array}
  \right ]_{\textit{point-tangent}}
\]

이것의 한 가지 단점은—그래픽에서 눈치챘을 수 있듯이—전체 곡선의 첫 번째와 마지막 점이 실제로 나머지 곡선과 결합되지 않는다는 것입니다. 각각 이전/다음 점이 없으므로 접선이 무엇이어야 하는지 계산할 방법이 없습니다. 또한 베지에 곡선에 대해 할 수 있었던 것처럼 세 점에 Catmull-Rom 곡선을 맞추는 것이 상당히 까다롭습니다. 그것에 대한 자세한 내용은 [다음 섹션](#catmullfitting)에서.

사실 계속하기 전에 이러한 곡선의 기본 형식을 실제로 그리는 방법을 살펴봅시다(기본이라고 말하는데, [상당히](https://en.wikipedia.org/wiki/Centripetal_Catmull%E2%80%93Rom_spline#Definition) 더 [복잡](https://en.wikipedia.org/wiki/Kochanek%E2%80%93Bartels_spline)하게 만드는 여러 변형이 있기 때문입니다).

```
tension = 0보다 큰 어떤 값, 기본값 1
points = 최소 4개의 좌표 목록

for p = 1 to points.length-3 (포함):
       p0 = points[p-1]
  v1 = p1 = points[p]
  v2 = p2 = points[p+1]
       p3 = points[p+2]

  s = 2 * tension
  dv1 = (p2-p0) / s
  dv2 = (p3-p1) / s

  for t = 0 to 1 (포함):
    c0 = 2*t^3 - 3*t^2 + 1,
    c1 = t^3 - 2*t^2 + t,
    c2 = -2*t^3 + 3*t^2,
    c3 = t^3 - t^2
    point(c0 * v1 + c1 * dv1 + c2 * v2 + c3 * dv2)
```

이제 Catmull-Rom 곡선은 [3차 Hermite 스플라인](https://en.wikipedia.org/wiki/Cubic_Hermite_spline)의 형태이고 3차 베지에 곡선도 _또한_ 3차 Hermite 스플라인의 형태이므로, 흥미로운 수학 프로그래밍에 부딪칩니다. 하나를 다른 것으로, 그리고 다시 변환할 수 있으며, 그렇게 하기 위한 수학은 놀랍도록 간단합니다!

Catmull-Rom 곡선과 베지에 곡선의 주요 차이점은 "점이 의미하는 것"입니다.

- 3차 베지에 곡선은 시작점, 시작점에서 접선을 암시하는 제어점, 끝점에서 접선을 암시하는 제어점, 끝점, 그리고 곡선 위의 좌표를 얻기 위해 점 벡터에 곱할 수 있는 특성 행렬로 정의됩니다.
- Catmull-Rom 곡선은 시작점, 그 시작점에 대한 접선, 끝점, 그 끝점에 대한 접선, 그리고 곡선 위의 좌표를 얻기 위해 점 벡터에 곱할 수 있는 특성 행렬로 정의됩니다.

이것들은 _매우_ 유사하므로 정확히 _얼마나_ 유사한지 봅시다. 이미 베지에 곡선에 대한 행렬 형식을 봤으니, Catmull-Rom 곡선에 대한 행렬 형식은 얼마나 다를까요?

\[
  \textit{CatmullRom}(t) =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   1 &  0 &  0 &  0 \\
   0 &  0 &  1 &  0 \\
  -3 &  3 & -2 & -1 \\
   2 & -2 &  1 &  1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  V_1 \\ V_2 \\ V'_1 \\ V'_2
  \end{bmatrix}
\]

꽤 엄청나게 유사합니다. 그렇다면 질문은: Catmull-Rom 행렬과 벡터를 사용한 그 식을 베지에 행렬과 벡터의 식으로 어떻게 변환할 수 있을까요? 짧은 답은 물론 "선형 대수학을 사용하여"이지만, 긴 답은 이 섹션의 나머지 부분이며 신경 쓰지 않을 수도 있는 약간의 수학이 포함됩니다. 두 곡선 형식 사이의 (믿을 수 없을 정도로 간단한) 변환만 알고 싶다면 다음 설명의 끝으로 건너뛰어도 되지만, 하나에서 다른 것을 _어떻게_ 얻을 수 있는지 원한다면... 수학을 해봅시다!

<div class="note">

## 변환 공식 도출

Catmull-Rom 곡선과 베지에 곡선 사이를 변환하려면 두 가지를 알아야 합니다. 첫째, 좌표와 접선의 혼합이 아니라 "4개 좌표 집합"을 사용하여 Catmull-Rom 곡선을 표현하는 방법, 둘째, 그러한 Catmull-Rom 좌표를 베지에 형식으로 변환하는 방법과 변환하는 방법.

첫 번째 부분으로 시작하여, "어떤 행렬 **T**"를 적용하여 Catmull-Rom **V** 좌표에서 베지에 **P** 좌표로 어떻게 갈 수 있는지 알아봅시다. 그 **T**가 무엇인지 아직 모르지만 알아내겠습니다.

\[
  \begin{bmatrix}
  V_1 \\ V_2 \\ V'_1 \\ V'_2
  \end{bmatrix}
  =
  T
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
  =
  \begin{bmatrix}
  P_2 \\ P_3 \\ \frac{P_3 - P_1}{2} \\ \frac{P_4 - P_2}{2}
  \end{bmatrix}
\]

따라서 이 매핑은 Catmull-Rom "점 + 접선" 벡터를 "모든 좌표" 벡터를 기반으로 한 것에 매핑하기 위해 <em>T</em>를 적용하면 시작점으로 P2, 끝점으로 P3, 그리고 각각 P1과 P3, P2와 P4 사이의 선을 기반으로 한 두 접선을 산출하도록 매핑 행렬을 결정해야 한다고 말합니다.

<em>T</em>를 계산하는 것은 실제로 더 많은 "숫자 배열"입니다.

\[
  T
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
  =
  \begin{bmatrix}
  P_2 \\ P_3 \\ \frac{P_3 - P_1}{2} \\ \frac{P_4 - P_2}{2}
  \end{bmatrix}
  =
  \begin{bmatrix}
   0 \cdot P1 &+ 1 \cdot P2 &+ 0 \cdot P3 &+ 0 \cdot P4 \\
   0 \cdot P1 &+ 0 \cdot P2 &+ 1 \cdot P3 &+ 0 \cdot P4 \\
  \frac{-1}{2} \cdot P1 &+ 0 \cdot P2 &+ \frac{1}{2} \cdot P3 &+ 0 \cdot P4 \\
   0 \cdot P1 & \frac{-1}{2} \cdot P2 &+ 0 \cdot P3 &+ \frac{1}{2} \cdot P4
  \end{bmatrix}
  =
  \begin{bmatrix}
   0 &  1 & 0 & 0 \\
   0 &  0 & 1 & 0 \\
  \frac{-1}{2} &  0 & \frac{1}{2} & 0 \\
   0 & \frac{-1}{2} & 0 & \frac{1}{2}
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

따라서:

\[
  T
  =
  \begin{bmatrix}
   0 &  1 & 0 & 0 \\
   0 &  0 & 1 & 0 \\
  \frac{-1}{2} &  0 & \frac{1}{2} & 0 \\
   0 & \frac{-1}{2} & 0 & \frac{1}{2}
  \end{bmatrix}
\]

그러나 _완전히_ 끝나지 않았습니다. Catmull-Rom 곡선에는 τ(소문자 "tau")로 작성된 "장력" 매개변수가 있으며, 이것은 접선 벡터의 확대/축소 인수입니다. 장력이 클수록 접선이 작아지고, 장력이 작을수록 접선이 커집니다. 따라서 장력 인수는 접선의 분모에 들어가며, 계속하기 전에 좌표 벡터 표현과 매핑 행렬 <em>T</em> 모두에 그 장력 인수를 추가해봅시다.

\[
  \begin{bmatrix}
  V_1 \\ V_2 \\ V'_1 \\ V'_2
  \end{bmatrix}
  =
  \begin{bmatrix}
  P_2 \\ P_3 \\ \frac{P_3 - P_1}{2τ} \\ \frac{P_4 - P_2}{2τ}
  \end{bmatrix}
  ,\
  T
  =
  \begin{bmatrix}
   0 &  1 & 0 & 0 \\
   0 &  0 & 1 & 0 \\
  \frac{-1}{2τ} &  0 & \frac{1}{2τ} & 0 \\
   0 & \frac{-1}{2τ} & 0 & \frac{1}{2τ}
  \end{bmatrix}
\]

매핑 행렬이 제대로 완료되었으므로, "점 + 접선" Catmull-Rom 행렬 형식을 4개 좌표 측면의 행렬 형식으로 다시 쓰고 무엇을 얻는지 봅시다.

\[
  \textit{CatmullRom}(t)
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   1 &  0 &  0 &  0 \\
   0 &  0 &  1 &  0 \\
  -3 &  3 & -2 & -1 \\
   2 & -2 &  1 &  1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  V_1 \\ V_2 \\ V'_1 \\ V'_2
  \end{bmatrix}
\]

점/접선 벡터를 모든 좌표 식으로 바꿉니다.

\[
  \textit{CatmullRom}(t)
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   1 &  0 &  0 &  0 \\
   0 &  0 &  1 &  0 \\
  -3 &  3 & -2 & -1 \\
   2 & -2 &  1 &  1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   0 &  1 & 0 & 0 \\
   0 &  0 & 1 & 0 \\
  \frac{-1}{2τ} &  0 & \frac{1}{2τ} & 0 \\
   0 & \frac{-1}{2τ} & 0 & \frac{1}{2τ}
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

행렬을 병합합니다.

\[
  \textit{CatmullRom}(t)
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
    0 &    1 &       0 &  0 \\
   \frac{-1}{2τ} &    0 &  \frac{1}{2τ} &  0 \\
   \frac{1}{τ} &  \frac{1}{2t} - 3 & 3 - \frac{1}{t} & \frac{-1}{2t} \\
   \frac{-1}{2t} &  2 - \frac{1}{2τ} & \frac{1}{2τ} - 2 &  \frac{1}{2t}
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

이것은 베지에 행렬 형식과 매우 비슷해 보이며, 베지에 곡선 챕터에서 본 것처럼 다음과 같아야 합니다.

\[
  \textit{Bézier}(t)
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  1 &  0 &  0 & 0 \\
  -3 &  3 &  0 & 0 \\
  3 & -6 &  3 & 0 \\
  -1 &  3 & -3 & 1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

따라서 베지에 곡선을 사용하여 Catmull-Rom 곡선을 표현하려면 이 Catmull-Rom 비트를 바꿔야 합니다.

\[
  \begin{bmatrix}
    0 &    1 &       0 &  0 \\
   \frac{-1}{2τ} &    0 &  \frac{1}{2τ} &  0 \\
   \frac{1}{τ} &  \frac{1}{2t} - 3 & 3 - \frac{1}{t} & \frac{-1}{2t} \\
   \frac{-1}{2t} &  2 - \frac{1}{2τ} & \frac{1}{2τ} - 2 &  \frac{1}{2t}
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

다음과 같이 보이는 것으로:

\[
  \begin{bmatrix}
  1 &  0 &  0 & 0 \\
  -3 &  3 &  0 & 0 \\
  3 & -6 &  3 & 0 \\
  -1 &  3 & -3 & 1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

그리고 그렇게 하는 방법은 상당히 간단한 행렬 재작성입니다. 보장해야 하는 동등성으로 시작합니다.

\[
  \begin{bmatrix}
    0 &    1 &       0 &  0 \\
  \frac{-1}{2τ} &    0 &  \frac{1}{2τ} &  0 \\
  \frac{1}{τ} &  \frac{1}{2t} - 3 & 3 - \frac{1}{t} & \frac{-1}{2t} \\
  \frac{-1}{2t} &  2 - \frac{1}{2τ} & \frac{1}{2τ} - 2 &  \frac{1}{2t}
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
  =
  \begin{bmatrix}
  1 &  0 &  0 & 0 \\
  -3 &  3 &  0 & 0 \\
  3 & -6 &  3 & 0 \\
  -1 &  3 & -3 & 1
  \end{bmatrix}
  \cdot
  V
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

그런 다음 동등성에 영향을 미치지 않고 양쪽에서 좌표 벡터를 제거할 수 있습니다.

\[
    \begin{bmatrix}
      0 &    1 &       0 &  0 \\
    \frac{-1}{2τ} &    0 &  \frac{1}{2τ} &  0 \\
    \frac{1}{τ} &  \frac{1}{2t} - 3 & 3 - \frac{1}{t} & \frac{-1}{2t} \\
    \frac{-1}{2t} &  2 - \frac{1}{2τ} & \frac{1}{2τ} - 2 &  \frac{1}{2t}
    \end{bmatrix}
  =
    \begin{bmatrix}
    1 &  0 &  0 & 0 \\
    -3 &  3 &  0 & 0 \\
    3 & -6 &  3 & 0 \\
    -1 &  3 & -3 & 1
    \end{bmatrix}
    \cdot
    V
\]

그런 다음 베지에 행렬의 역행렬로 양쪽을 왼쪽 곱셈하여 오른쪽의 베지에 행렬을 "제거"할 수 있습니다.

\[
  {
  \begin{bmatrix}
   1 &  0 &  0 & 0 \\
  -3 &  3 &  0 & 0 \\
   3 & -6 &  3 & 0 \\
  -1 &  3 & -3 & 1
  \end{bmatrix}
  }^{-1}
  \cdot
  \begin{bmatrix}
    0 &    1 &       0 &  0 \\
   \frac{-1}{2τ} &    0 &  \frac{1}{2τ} &  0 \\
   \frac{1}{τ} &  \frac{1}{2t} - 3 & 3 - \frac{1}{t} & \frac{-1}{2t} \\
   \frac{-1}{2t} &  2 - \frac{1}{2τ} & \frac{1}{2τ} - 2 &  \frac{1}{2t}
  \end{bmatrix}
  =
  {
  \begin{bmatrix}
   1 &  0 &  0 & 0 \\
  -3 &  3 &  0 & 0 \\
   3 & -6 &  3 & 0 \\
  -1 &  3 & -3 & 1
  \end{bmatrix}
  }^{-1}
  \cdot
  \begin{bmatrix}
   1 &  0 &  0 & 0 \\
  -3 &  3 &  0 & 0 \\
   3 & -6 &  3 & 0 \\
  -1 &  3 & -3 & 1
  \end{bmatrix}
  \cdot
  V
\]

행렬 곱하기 그 역행렬은 1의 행렬 등가물이고, "무언가 곱하기 1"은 "무언가"와 같으므로 행렬/역행렬 쌍을 완전히 제거할 수 있습니다.

\[
  {
  \begin{bmatrix}
   1 &  0 &  0 & 0 \\
  -3 &  3 &  0 & 0 \\
   3 & -6 &  3 & 0 \\
  -1 &  3 & -3 & 1
  \end{bmatrix}
  }^{-1}
  \cdot
  \begin{bmatrix}
    0 &    1 &       0 &  0 \\
   \frac{-1}{2τ} &    0 &  \frac{1}{2τ} &  0 \\
   \frac{1}{τ} &  \frac{1}{2t} - 3 & 3 - \frac{1}{t} & \frac{-1}{2t} \\
   \frac{-1}{2t} &  2 - \frac{1}{2τ} & \frac{1}{2τ} - 2 &  \frac{1}{2t}
  \end{bmatrix}
  =
  V
\]

이제 <em>기본적으로</em> 끝났습니다. 이 두 행렬을 곱하기만 하면 <em>V</em>가 무엇인지 알 수 있습니다.

\[
  \begin{bmatrix}
  0 & 1 & 0 & 0 \\
  \frac{-1}{6τ} & 1 & \frac{1}{6τ} & 0 \\
  0 & \frac{1}{6τ} & 1 & \frac{-1}{6τ} \\
  0 & 0 & 1 & 0
  \end{bmatrix}
  =
  V
\]

이제 함수 퍼즐의 마지막 조각이 생겼습니다. 각 단계를 실행해봅시다.

1. Catmull-Rom 함수로 시작:

\[
  \textit{CatmullRom}(t)
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   1 &  0 &  0 &  0 \\
   0 &  0 &  1 &  0 \\
  -3 &  3 & -2 & -1 \\
   2 & -2 &  1 &  1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  V_1 \\ V_2 \\ V'_1 \\ V'_2
  \end{bmatrix}
\]

2. 순수 좌표 형식으로 재작성:

\[
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   1 &  0 &  0 &  0 \\
   0 &  0 &  1 &  0 \\
  -3 &  3 & -2 & -1 \\
   2 & -2 &  1 &  1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_2 \\ P_3 \\ \frac{P_3 - P_1}{2τ} \\ \frac{P_4 - P_2}{2τ}
  \end{bmatrix}
\]

3. "일반" 좌표 벡터에 대해 재작성:

\[
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   1 &  0 &  0 &  0 \\
   0 &  0 &  1 &  0 \\
  -3 &  3 & -2 & -1 \\
   2 & -2 &  1 &  1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   0 &  1 & 0 & 0 \\
   0 &  0 & 1 & 0 \\
  \frac{-1}{2τ} &  0 & \frac{1}{2τ} & 0 \\
   0 & \frac{-1}{2τ} & 0 & \frac{1}{2τ}
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

4. 내부 행렬 병합:

\[
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
    0 &    1 &       0 &  0 \\
   \frac{-1}{2τ} &    0 &  \frac{1}{2τ} &  0 \\
   \frac{1}{τ} &  \frac{1}{2t} - 3 & 3 - \frac{1}{t} & \frac{-1}{2t} \\
   \frac{-1}{2t} &  2 - \frac{1}{2τ} & \frac{1}{2τ} - 2 &  \frac{1}{2t}
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

5. 베지에 행렬 형식으로 재작성:

\[
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   1 &  0 &  0 & 0 \\
  -3 &  3 &  0 & 0 \\
   3 & -6 &  3 & 0 \\
  -1 &  3 & -3 & 1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  0 & 1 & 0 & 0 \\
  \frac{-1}{6τ} & 1 & \frac{1}{6τ} & 0 \\
  0 & \frac{1}{6τ} & 1 & \frac{-1}{6τ} \\
  0 & 0 & 1 & 0
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3 \\ P_4
  \end{bmatrix}
\]

6. 그리고 "순수" 베지에 식이 되도록 좌표를 변환:

\[
  =
  \begin{bmatrix}
  1 & t & t^2 & t^3
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   1 &  0 &  0 & 0 \\
  -3 &  3 &  0 & 0 \\
   3 & -6 &  3 & 0 \\
  -1 &  3 & -3 & 1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_2 \\
  P_2 + \frac{P_3-P_1}{6 \cdot τ} \\
  P_3 - \frac{P_4-P_2}{6 \cdot τ} \\
  P_3
  \end{bmatrix}
\]

끝났습니다. 마침내 이 두 곡선을 변환하는 방법을 알게 되었습니다!

</div>

4개의 좌표 P<sub>1</sub> ~ P<sub>4</sub>로 정의된 Catmull-Rom 곡선이 있다면, 다음 벡터를 가진 베지에 곡선을 사용하여 그 곡선을 그릴 수 있습니다.

\[
  \begin{bmatrix}
  P_1 \\
  P_2 \\
  P_3 \\
  P_4
  \end{bmatrix}_{\textit{CatmullRom}}
  \Rightarrow
  \begin{bmatrix}
  P_2 \\
  P_2 + \frac{P_3-P_1}{6 \cdot τ} \\
  P_3 - \frac{P_4-P_2}{6 \cdot τ} \\
  P_3
  \end{bmatrix}_{\textit{Bézier}}
\]

마찬가지로 4개의 좌표 P<sub>1</sub> ~ P<sub>4</sub>로 정의된 베지에 곡선이 있다면, 다음 좌표 값을 가진 표준 장력 Catmull-Rom 곡선을 사용하여 그릴 수 있습니다.

\[
  \begin{bmatrix}
  P_1 \\
  P_2 \\
  P_3 \\
  P_4
  \end{bmatrix}_{\textit{Bézier}}
  \Rightarrow
  \begin{bmatrix}
  P_1 \\
  P_4 \\
  P_4 + 3(P_1 - P_2) \\
  P_1 + 3(P_4 - P_3)
  \end{bmatrix}_{\textit{CatmullRom}}
\]

또는 API가 일반 좌표를 사용하여 Catmull-Rom 곡선을 지정할 수 있는 경우:

\[
  \begin{bmatrix}
  P_1 \\
  P_2 \\
  P_3 \\
  P_4
  \end{bmatrix}_{\textit{Bézier}}
  \Rightarrow
  \begin{bmatrix}
  P_4 + 6(P_1 - P_2) \\
  P_1 \\
  P_4 \\
  P_1 + 6(P_4 - P_3)
  \end{bmatrix}_{\textit{CatmullRom}}
\]
