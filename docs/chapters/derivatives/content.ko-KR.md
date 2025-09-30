# 도함수

베지에 곡선의 도함수를 기반으로 할 수 있는 유용한 작업이 여러 가지 있는데, 베지에 곡선에 대한 재미있는 관찰 중 하나는 그 도함수도 실제로 베지에 곡선이라는 것입니다. 사실 베지에 곡선의 미분은 상대적으로 간단하지만, 약간의 수학이 필요합니다.

먼저 베지에 곡선의 도함수 규칙을 살펴봅시다.

\[
  \textit{Bézier}'(n,t) = n \cdot \sum_{i=0}^{n-1} (b_{i+1}-b_i) \cdot \textit{Bézier}(n-1,t)_i
\]

이것은 다음과 같이 쓸 수도 있습니다(이 공식의 <i>b</i>가 우리 <i>w</i> 가중치와 같고, <i>n</i>을 곱한 합은 각 항에 <i>n</i>을 곱한 합과 같다는 것을 관찰하면).

\[
  \textit{Bézier}'(n,t) = \sum_{i=0}^{n-1} \textit{Bézier}(n-1,t)_i \cdot n \cdot (w_{i+1}-w_i)
\]

즉, 평문으로 말하면: n차 베지에 곡선의 도함수는 (n-1)차 베지에 곡선이며, 항이 하나 적고, 새로운 가중치 w'<sub>0</sub>...w'<sub>n-1</sub>는 원래 가중치에서 n(w<sub>i+1</sub> - w<sub>i</sub>)로 도출됩니다. 따라서 가중치가 4개인 3차 곡선의 경우 도함수는 새로운 가중치 3개를 가집니다: w'<sub>0</sub> = 3(w<sub>1</sub>-w<sub>0</sub>), w'<sub>1</sub> = 3(w<sub>2</sub>-w<sub>1</sub>), w'<sub>2</sub> = 3(w<sub>3</sub>-w<sub>2</sub>).

<div class="note">

### "잠깐, 왜 그게 사실인가요?"

때로는 그냥 "이것이 도함수다"라고 들으면 좋지만, 왜 실제로 그런지 보고 싶을 수도 있습니다. 그래서 이 도함수에 대한 증명을 살펴봅시다. 먼저 가중치는 전체 베지에 함수와 독립적이므로 도함수는 다항식 기저 함수의 도함수만 포함합니다. 그럼 그것을 찾아봅시다.

\[
  B_{n,k}(t) \frac{d}{dt} = {n \choose k} t^k (1-t)^{n-k} \frac{d}{dt}
\]

[곱셈](https://en.wikipedia.org/wiki/Product_rule)과 [연쇄](https://en.wikipedia.org/wiki/Chain_rule) 규칙을 적용하면:

\[
\begin{array}{l}
  ... = {n \choose k} \left (
    k \cdot t^{k-1} (1-t)^{n-k} + t^k \cdot (1-t)^{n-k-1} \cdot (n-k) \cdot -1
  \right )
\end{array}
\]

다루기 어려우니 제대로 전개해봅시다.

\[
\begin{array}{l}
  ... = \frac{kn!}{k!(n-k)!} t^{k-1} (1-t)^{n-k} - \frac{(n-k)n!}{k!(n-k)!} t^k (1-t)^{n-1-k}
\end{array}
\]

이제 요령은 이 식을 다시 이항 계수가 있는 것으로 만드는 것입니다. 그러니까 "x!을 y!(x-y)!로 나눈 것"처럼 보이는 것으로 만들고 싶습니다. <i>n-1</i>과 <i>k-1</i> 항을 포함하는 방식으로 그렇게 할 수 있다면 올바른 방향으로 가는 것입니다.

\[
\begin{array}{l}
  ... = \frac{n!}{(k-1)!(n-k)!} t^{k-1} (1-t)^{n-k} - \frac{(n-k)n!}{k!(n-k)!} t^k (1-t)^{n-1-k} \\

  ... = n \left (
    \frac{(n-1)!}{(k-1)!(n-k)!} t^{k-1} (1-t)^{n-k} - \frac{(n-k)(n-1)!}{k!(n-k)!} t^k (1-t)^{n-1-k}
  \right ) \\

  ... = n \left (
    \frac{(n-1)!}{(k-1)!((n-1)-(k-1))!} t^{(k-1)} (1-t)^{(n-1)-(k-1)} - \frac{(n-1)!}{k!((n-1)-k)!} t^k (1-t)^{(n-1)-k}
  \right )
\end{array}
\]

첫 번째 부분이 완료되었습니다. 괄호 안의 두 성분은 실제로 정규적이고 저차인 베지에 식입니다.

\[\begin{array}{l}
  ... = n \left (
    \frac{x!}{y!(x-y)!} t^{y} (1-t)^{x-y} - \frac{x!}{k!(x-k)!} t^k (1-t)^{x-k}
  \right )
  ~,~with~x=n-1,~y=k-1
  \\
  ... = n \left ( B_{(n-1),(k-1)}(t) - B_{(n-1),k}(t) \right )
\end{array}
\]

이제 이것을 가중치 베지에 곡선에 적용해봅시다. 앞에서 본 일반 곡선 공식을 쓴 다음 도함수까지 진행해봅시다.

\[\begin{array}{lcl}
  \textit{Bézier}_{n,k}(t) &=& B_{n,0}(t) \cdot w_0 + B_{n,1}(t) \cdot w_1 + B_{n,2}(t) \cdot w_2 + B_{n,3}(t) \cdot w_3 + ... \\
  \textit{Bézier}_{n,k}(t) \frac{d}{dt} &=& n \cdot (B_{n-1,-1}(t) - B_{n-1,0}(t)) \cdot w_0 + \\
                               & & n \cdot (B_{n-1,0}(t) - B_{n-1,1}(t)) \cdot w_1 + \\
                               & & n \cdot (B_{n-1,1}(t) - B_{n-1,2}(t)) \cdot w_2 + \\
                               & & n \cdot (B_{n-1,2}(t) - B_{n-1,3}(t)) \cdot w_3 + \\
                               & & ...
\end{array}\]

이것을 전개하고(항이 어떻게 정렬되는지 보여주기 위해 색을 사용함) <i>k</i> 값이 증가하는 순서로 항을 재정렬하면 다음과 같습니다.

\[\begin{array}{lclc}
  n \cdot B_{n-1,-1}(t) \cdot w_0 &+& & \\
  n \cdot B_{n-1,BLUE[0]}(t) \cdot w_1 &-& n \cdot B_{n-1,BLUE[0]}(t) \cdot w_0 & + \\
  n \cdot B_{n-1,RED[1]}(t) \cdot w_2 &-& n \cdot B_{n-1,RED[1]}(t) \cdot w_1 & + \\
  n \cdot B_{n-1,MAGENTA[2]}(t) \cdot w_3 &-& n \cdot B_{n-1,MAGENTA[2]}(t) \cdot w_2 & + \\
  ... &-& n \cdot B_{n-1,3}(t) \cdot w_3 & + \\
  ... & & &
\end{array}\]

이 중 두 항은 사라집니다. 첫 번째 항은 합에 -1번째 항이 없기 때문에 사라집니다. 따라서 항상 "아무것도" 기여하지 않으므로 도함수 함수를 찾는 목적으로 완전히 무시할 수 있습니다. 다른 항은 이 전개의 맨 마지막 항입니다. <i>B<sub>n-1,n</sub></i>와 관련된 항입니다. 이 항은 [<i>i</i> choose <i>i+1</i>]의 이항 계수를 가지는데, 이것은 존재하지 않는 이항 계수입니다. 다시 말해 이 항도 "아무것도" 기여하지 않으므로 무시할 수 있습니다. 따라서 다음만 남습니다.

\[\begin{array}{lclc}
  n \cdot B_{n-1,BLUE[0]}(t) \cdot w_1 &-& n \cdot B_{n-1,BLUE[0]}(t) \cdot w_0 &+ \\
  n \cdot B_{n-1,RED[1]}(t) \cdot w_2 &-& ~n \cdot B_{n-1,RED[1]}(t) \cdot w_1 &+ \\
  n \cdot B_{n-1,MAGENTA[2]}(t) \cdot w_3 &-& n \cdot B_{n-1,MAGENTA[2]}(t) \cdot w_2 &+ \\
  ...
\end{array}\]

그리고 이것은 그냥 저차 곡선들의 합입니다.

\[
  \textit{Bézier}_{n,k}(t) \frac{d}{dt} = n \cdot B_{(n-1),BLUE[0]}(t) \cdot (w_1 - w_0)
                            + n \cdot B_{(n-1),RED[1]}(t) \cdot (w_2 - w_1)
                            + n \cdot B_{(n-1),MAGENTA[2]}(t) \cdot (w_3 - w_2)
                            ~+ ~...
\]

이것을 일반 합으로 다시 쓰면 끝입니다.

\[
  \textit{Bézier}_{n,k}(t) \frac{d}{dt} = \sum_{k=0}^{n-1} n \cdot B_{n-1,k}(t) \cdot (w_{k+1} - w_k)
                               = \sum_{k=0}^{n-1} B_{n-1,k}(t) \cdot \underset{\textit{도함수 가중치}}
                                 {\underbrace{n \cdot (w_{k+1} - w_k)}}
\]

</div>

원래 공식과 유사한 형태로 다시 작성해서 차이를 볼 수 있도록 하겠습니다. 먼저 베지에 곡선의 원래 공식을 나열한 다음 도함수를 나열하겠습니다.

\[
  \textit{Bézier}(n,t) = \sum_{i=0}^{n}
                \underset{\textit{이항 항}}{\underbrace{\binom{n}{i}}}
                \cdot\
                \underset{\textit{다항식 항}}{\underbrace{(1-t)^{n-i} \cdot t^{i}}}
                \cdot\
                \underset{\textit{가중치}}{\underbrace{w_i}}
\]

\[
  \textit{Bézier}'(n,t) = \sum_{i=0}^{k}
                \underset{\textit{이항 항}}{\underbrace{\binom{k}{i}}}
                \cdot\
                \underset{\textit{다항식 항}}{\underbrace{(1-t)^{k-i} \cdot t^{i}}}
                \cdot\
                \underset{\textit{도함수 가중치}}{\underbrace{n \cdot (w_{i+1} - w_i)}}
                ~,~\textit{with } k=n-1
\]

차이점이 뭘까요? 실제 베지에 곡선 측면에서는 사실상 아무것도 없습니다! 차수를 낮췄지만(<i>n</i>이 아니라 이제 <i>n-1</i>), 여전히 같은 베지에 함수입니다. 유일한 실제 차이는 곡선의 함수를 미분할 때 가중치가 어떻게 변하는지입니다. A, B, C, D 네 점이 있으면 도함수는 세 점을 가지고, 2차 도함수는 두 점, 3차 도함수는 한 점을 가집니다.

\[ \begin{array}{llll}
  B(n,t),    &        & w = \{A,B,C,D\} \\
  B'(n,t),   & n = 3, & w' = \{A',B',C'\}    &= \{3 \cdot (B-A), {~} 3 \cdot (C-B), {~} 3 \cdot (D-C)\} \\
  B''(n,t),  & n = 2, & w'' = \{A'',B''\}    &= \{2 \cdot (B'-A'), {~} 2 \cdot (C'-B')\} \\
  B'''(n,t), & n = 1, & w''' = \{A'''\} &= \{1 \cdot (B''-A'')\}
\end{array} \]

가중치가 하나 이상 남아 있는 한 이 트릭을 계속 수행할 수 있습니다. 가중치가 하나 남으면 다음 단계에서 <i>k = 0</i>이 되고, "베지에 함수" 합의 결과는 0입니다. 아무것도 더하지 않기 때문이죠. 따라서 2차 곡선에는 2차 도함수가 없고, 3차 곡선에는 3차 도함수가 없으며, 일반화하면: <i>n</i>차 곡선은 <i>n-1</i>개의 (의미 있는) 도함수를 가지며, 그 이상의 도함수는 0입니다.
