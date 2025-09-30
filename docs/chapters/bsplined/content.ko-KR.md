# B-Spline 도함수

B-Spline에 특정한 마지막 섹션: 베지에 곡선에 대해 살펴본 것과 동일한 절차를 B-Spline에 적용하려면 1차 및 2차 도함수를 알아야 합니다. 그런데... B-Spline의 도함수는 무엇일까요?

다행히도 베지에 곡선의 경우와 마찬가지로 B-Spline의 도함수는 그 자체로 (저차) B-Spline입니다. 다음 두 함수는 <em>n</em>개의 점과 길이 <em>d+n+1</em>의 노트 벡터를 가진 차수 <em>d</em>의 B-Spline에 대한 일반 B-Spline 공식과 그 도함수를 지정합니다.

\[
  C(t) = \sum_{i=0}^n P_i \cdot N_{i,k}(t)
\]

\[
  C'(t) = \sum_{i=0}^{n-1} P_i \prime \cdot N_{i+1,k-1}(t)
\]

여기서

\[
  P_i \prime = \frac{d}{\textit{knot}_{i+d+1} - \textit{knot}_{i+1}} (P_{i+1} - P_i)
\]

따라서 베지에 도함수와 마찬가지로 보간된 가중치를 가진 새로운 보간 함수일 뿐인 도함수 함수를 볼 수 있습니다. 이 정보를 사용하면 접선과 법선을 그리고, 곡률 함수를 결정하고, 변곡점을 그리는 등 모든 멋진 작업을 수행할 수 있습니다.

구체적인 예로, 5개의 좌표를 가진 3차(=차수 3) B-Spline과 길이 3 + 5 + 1 = 9의 균일 노트 벡터를 살펴봅시다.

\[
  \begin{array}{l}
    d = 3, \\
    P = {(50,240), (185,30), (320,135), (455,25), (560,255)}, \\
    \textit{knots} = {0,1,2,3,4,5,6,7,8}
  \end{array}
\]

<BSplineGraphic sketch={require('./demonstrator')} />

위의 지식을 적용하면 4개의 점 <em>P'</em>을 가진 차수 <em>d-1</em>의 새로운 B-Spline이 됩니다.

\[
  \begin{array}{l}
    P_0 \prime = \frac{d}{\textit{knot}_{i+d+1} - \textit{knot}_{i+1}} (P_{i+1} - P_i)
    = \frac{3}{\textit{knot}_{4} - \textit{knot}_{1}} (P_1 - P_0)
    = \frac{3}{3} (P_1 - P_0)
    = (135, -210) \\
    P_1 \prime = \frac{d}{\textit{knot}_{i+d+1} - \textit{knot}_{i+1}} (P_{i+1} - P_i)
    = \frac{3}{\textit{knot}_{5} - \textit{knot}_{2}} (P_2 - P_1)
    = \frac{3}{3} (P_2 - P_1)
    = (135, 105) \\
    P_2 \prime = \frac{d}{\textit{knot}_{i+d+1} - \textit{knot}_{i+1}} (P_{i+1} - P_i)
    = \frac{3}{\textit{knot}_{6} - \textit{knot}_{3}} (P_3 - P_2)
    = \frac{3}{3} (P_3 - P_2)
    = (135, -110) \\
    P_3 \prime = \frac{d}{\textit{knot}_{i+d+1} - \textit{knot}_{i+1}} (P_{i+1} - P_i)
    = \frac{3}{\textit{knot}_{7} - \textit{knot}_{4}} (P_4 - P_3)
    = \frac{3}{3} (P_4 - P_3)
    = (105, 230) \\
  \end{array}
\]

따라서 다음 매개변수를 가진 도함수로 끝납니다.

\[
  \begin{array}{l}
    d = 3, \\
    P = {(50,240), (185,30), (320,135), (455,25), (560,255)}, \\
    \textit{knots} = {0,1,2,3,4,5,6,7,8}
  \end{array}
\]

<BSplineGraphic sketch={require('./derived')} />
