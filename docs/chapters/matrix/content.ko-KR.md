# 행렬 연산으로서의 베지에 곡률

베지에 공식을 다항식 기저 함수와 계수 행렬로 표현하고 실제 좌표를 행렬로 표현하여 베지에 곡선을 행렬 연산으로 나타낼 수도 있습니다. P<sub>...</sub>를 사용하여 "하나 이상의 차원에서" 좌표 값을 참조하여 3차 곡선에 대해 이것이 무엇을 의미하는지 살펴봅시다.

\[
B(t) = P_1 \cdot (1-t)^3 + P_2 \cdot 3 \cdot (1-t)^2 \cdot t + P_3 \cdot 3 \cdot (1-t) \cdot t^2 + P_4 \cdot t^3
\]

잠시 실제 좌표를 무시하면 다음이 있습니다.

\[
B(t) = (1-t)^3 + 3 \cdot (1-t)^2 \cdot t + 3 \cdot (1-t) \cdot t^2 + t^3
\]

이것을 네 개의 식의 합으로 쓸 수 있습니다.

\[
  \begin{matrix}
   ... & = & (1-t)^3 \\
     & + & 3 \cdot (1-t)^2 \cdot t \\
     & + & 3 \cdot (1-t) \cdot t^2 \\
     & + & t^3 \\
  \end{matrix}
\]

그리고 이러한 식을 전개할 수 있습니다.

\[
  \begin{matrix}
   ... & = & (1-t) \cdot (1-t) \cdot (1-t) & = & -t^3 + 3 \cdot t^2 - 3 \cdot t + 1 \\
     & + & 3 \cdot (1-t) \cdot (1-t) \cdot t & = & 3 \cdot t^3 - 6 \cdot t^2 + 3 \cdot t \\
     & + & 3 \cdot (1-t) \cdot t \cdot t & = & -3 \cdot t^3 + 3 \cdot t^2 \\
     & + & t \cdot t \cdot t & = & t^3 \\
  \end{matrix}
\]

또한 모든 1과 0 인수를 명시적으로 만들 수 있습니다.

\[
  \begin{matrix}
   ... & = & -1 \cdot t^3 + 3 \cdot t^2 - 3 \cdot t + 1 \\
     & + & +3 \cdot t^3 - 6 \cdot t^2 + 3 \cdot t + 0 \\
     & + & -3 \cdot t^3 + 3 \cdot t^2 + 0 \cdot t + 0 \\
     & + & +1 \cdot t^3 + 0 \cdot t^2 + 0 \cdot t + 0 \\
  \end{matrix}
\]

그리고 *그것*을 네 개의 행렬 연산 시리즈로 볼 수 있습니다.

\[
  \begin{bmatrix}t^3 & t^2 & t & 1\end{bmatrix} \cdot \begin{bmatrix}-1 \\ 3 \\ -3 \\ 1\end{bmatrix}
  + \begin{bmatrix}t^3 & t^2 & t & 1\end{bmatrix} \cdot \begin{bmatrix}3 \\ -6 \\ 3 \\ 0\end{bmatrix}
  + \begin{bmatrix}t^3 & t^2 & t & 1\end{bmatrix} \cdot \begin{bmatrix}-3 \\ 3 \\ 0 \\ 0\end{bmatrix}
  + \begin{bmatrix}t^3 & t^2 & t & 1\end{bmatrix} \cdot \begin{bmatrix}1 \\ 0 \\ 0 \\ 0\end{bmatrix}
\]

이것을 단일 행렬 연산으로 압축하면 다음을 얻습니다.

\[
  \begin{bmatrix}t^3 & t^2 & t & 1\end{bmatrix} \cdot \begin{bmatrix}
      -1 &  3 & -3 & 1 \\
       3 & -6 &  3 & 0 \\
      -3 &  3 &  0 & 0 \\
       1 &  0 &  0 & 0
    \end{bmatrix}
\]

이런 종류의 다항식 기저 표현은 일반적으로 기저가 증가하는 순서로 작성되므로 `t` 행렬을 가로로 뒤집고 큰 "혼합" 행렬을 거꾸로 뒤집어야 합니다.

\[
  \begin{bmatrix}1 & t & t^2 & t^3\end{bmatrix} \cdot \begin{bmatrix}
       1 &  0 &  0 & 0 \\
      -3 &  3 &  0 & 0 \\
       3 & -6 &  3 & 0 \\
      -1 &  3 & -3 & 1
    \end{bmatrix}
\]

그런 다음 마지막으로 원래 좌표를 단일 세 번째 행렬로 추가할 수 있습니다.

\[
  B(t) = \begin{bmatrix}
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

2차 곡선에 대해서도 같은 트릭을 수행할 수 있으며, 이 경우 다음으로 끝납니다.

\[
  B(t) = \begin{bmatrix}
  1 & t & t^2
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
   1 &  0 & 0 \\
  -2 &  2 & 0 \\
   1 & -2 & 1
  \end{bmatrix}
  \cdot
  \begin{bmatrix}
  P_1 \\ P_2 \\ P_3
  \end{bmatrix}
\]

`t` 값을 대입한 다음 행렬을 곱하면 원래 다항식 함수를 평가할 때나 점진적 선형 보간을 사용하여 곡선을 평가할 때와 정확히 같은 값을 얻습니다.

**그래서: 왜 행렬을 신경 써야 할까요?** 행렬 표현을 사용하면 함수에 대해 그렇지 않으면 알기 어려운 것들을 발견할 수 있습니다. 곡선이 [삼각 행렬](https://en.wikipedia.org/wiki/Triangular_matrix)을 형성하고 곡선에 사용하는 실제 좌표의 곱과 같은 행렬식을 가지는 것으로 밝혀졌습니다. 또한 역행렬이 가능하므로 모두 충족되는 [많은 속성](https://en.wikipedia.org/wiki/Invertible_matrix#The_invertible_matrix_theorem)이 있습니다. 물론 주요 질문은 "이것이 지금 왜 우리에게 유용한가?"이며, 답은 *즉시* 유용하지는 않지만 특정 곡선 속성을 함수 조작을 통해 계산하거나 행렬의 영리한 사용을 통해 계산할 수 있는 일부 인스턴스를 보게 될 것이며, 때로는 행렬 접근 방식이 (극적으로) 더 빠를 수 있다는 것입니다.

그러니 지금은 이런 방식으로 곡선을 나타낼 수 있다는 것만 기억하고 계속 진행합시다.
