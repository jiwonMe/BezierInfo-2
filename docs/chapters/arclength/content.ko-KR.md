# 호 길이

베지에 곡선은 얼마나 길까요? 알고 보니 이것은 실제로 쉬운 질문이 아닙니다. 답변에는 근 찾기처럼 일반적으로 전통적인 방식으로 풀 수 없는 수학이 필요하기 때문입니다. *f<sub>x</sub>(t)*와 *f<sub>y</sub>(t)*를 가진 매개변수 곡선이 있다면, 시작점부터 어떤 점 *t = z*까지 측정한 곡선의 길이는 다음과 같은 겉보기에 간단해 보이는(약간 압도적이지만) 공식을 사용하여 계산됩니다.

\[
  \int_{0}^{z}\sqrt{f_x'(t)^2+f_y'(t)^2} dt
\]

또는 Leibnitz 표기법을 사용하여 더 일반적으로 다음과 같이 씁니다.

\[
  \textit{length} = \int_{0}^{z}\sqrt{ \left (dx/dt \right )^2+\left (dy/dt \right )^2} dt
\]

이 공식은 매개변수 곡선의 길이가 실제로 직각 삼각형의 대각선을 계산하기 위한 피타고라스 정리와 놀랍도록 비슷해 보이는 함수 "아래"의 **면적**과 같다고 말합니다. 꽤 간단하게 들리죠? 슬프게도 간단하지 않습니다... 추격이 끝난 후 바로 설명하면: 2차 곡선의 경우 이 공식은 [다루기 어려운 계산](https://www.wolframalpha.com/input/?i=antiderivative+for+sqrt((2*(1-t)*t*B+%2B+t%5E2*C)%27%5E2+%2B+(2*(1-t)*t*E)%27%5E2)&incParTime=true)을 생성하며, 우리는 그런 방식으로 구현하지 않을 것입니다. 3차 베지에 곡선의 경우 상황이 더욱 재미있어집니다. "닫힌 형태" 해결책이 없기 때문입니다. 즉, 미적분이 작동하는 방식 때문에 호 길이를 계산할 수 있는 일반 공식이 없습니다. 꽤 중요하므로 반복하겠습니다. ***3차 및 그 이상의 베지에 곡선의 경우, "모든 가능한 좌표에 대해" 사용하려면 이 함수를 푸는 방법이 없습니다***.

진지하게: [할 수 없습니다](https://en.wikipedia.org/wiki/Abel%E2%80%93Ruffini_theorem).

따라서 다시 수치적 접근 방식으로 전환합니다. 여기서 살펴볼 방법은 [Gauss 구적법](https://www.youtube.com/watch?v=unWguclP-Ds&feature=BFa&list=PLC8FC40C714F5E60F&index=1)입니다. 이 근사는 정말 깔끔한 트릭입니다. 모든 *n차* 다항식에 대해 적분에 대한 근사 값을 정말 효율적으로 찾기 때문입니다. 이 절차를 길게 설명하는 것은 이 페이지의 범위를 훨씬 벗어나므로 작동 원리에 관심이 있다면 이 단락에 링크된 University of South Florida의 절차에 대한 비디오 강의를 추천할 수 있습니다. 우리가 찾고 있는 일반 해결책은 다음과 같습니다.

\[
  \int_{-1}^{1}\sqrt{ \left (dx/dt \right )^2+\left (dy/dt \right )^2} dt
  =
  \int_{-1}^{1}f(t) dt
  \simeq
  \left [
    \underset{\textit{strip 1}}{ \underbrace{ C_1 \cdot f\left(t_1\right) }}
    ~+~...
    ~+~\underset{\textit{strip n}}{ \underbrace{ C_n \cdot f\left(t_n\right) }}
  \right ]
  =
  \underset{\textit{strips 1 through n}}{
    \underbrace{
      \sum_{i=1}^{n}{
        C_i \cdot f\left(t_i\right)
      }
    }
  }
\]

평문으로: 적분 함수는 항상 함수의 그래프 아래에 "앉아 있는" (무한히 많은) (무한히 얇은) 직사각형 스트립의 합으로 취급될 수 있습니다. 이 아이디어를 설명하기 위해 다음 그래프는 사인파 함수에 대한 적분을 보여줍니다. 더 많은 스트립을 사용할수록(물론 더 많이 사용할수록 더 얇아짐) 곡선 아래의 진정한 면적에 더 가까워지고, 따라서 근사가 더 좋아집니다.

<div class="figure">
  <graphics-element title="함수의 근사 적분" src="./draw-slices.js" data-steps="10"></graphics-element>
  <graphics-element title="더 나은 근사" src="./draw-slices.js" data-steps="24"></graphics-element>
  <graphics-element title="훨씬 더 나은 근사" src="./draw-slices.js" data-steps="99"></graphics-element>
</div>

이제 무한히 많은 항을 합산하고 무한히 얇은 직사각형은 컴퓨터가 작업할 수 있는 것이 아니므로 대신 유한한 수의 "그냥 얇은" 직사각형 스트립의 합을 사용하여 무한 합을 근사할 것입니다. 충분히 많은 수의 충분히 얇은 직사각형 스트립을 사용하는 한, 이것은 실제 값에 꽤 가까운 근사를 제공합니다.

따라서 트릭은 유용한 직사각형 스트립을 찾는 것입니다. 순진한 방법은 모두 같은 너비를 가진 *n*개의 스트립을 단순히 만드는 것이지만, 몇 개의 스트립을 사용할지 나타내는 *n* 값에 따라 *C*와 *f(t)*에 대한 특수 값을 사용하는 훨씬 더 나은 방법이 있으며, 이것을 Legendre-Gauss 구적법이라고 합니다.

이 접근 방식은 균등하게 간격을 두지 *않고* 대신 함수를 다항식으로 설명하는 것을 기반으로 특별한 방식으로 간격을 두는 스트립을 사용하며(스트립이 많을수록 다항식이 더 정확함), 그런 다음 그 다항식에 대한 정확한 적분을 계산합니다. 본질적으로 평탄화된 곡선에서 호 길이 계산을 수행하고 있지만, Legendre-Gauss 솔루션이 지시하는 간격을 기반으로 평탄화하고 있습니다.

<div class="note">

사용할 접근 방식의 한 가지 요구 사항은 적분이 -1에서 1까지 실행되어야 한다는 것입니다. 베지에 곡선을 다루고 있고 곡선 섹션의 길이는 0에서 "1보다 작거나 같은 어떤 값"(그 값을 *z*라고 부르겠습니다)까지 실행되는 값에 적용되므로 좋지 않습니다. 다행히도 입력을 이동하고 확대/축소하여 적분 구간을 다른 적분 구간으로 상당히 쉽게 변환할 수 있습니다. 그렇게 하면 다음을 얻습니다.

\[
\begin{array}{l}
  \int_{0}^{z}\sqrt{ \left (dx/dt \right )^2+\left (dy/dt \right )^2} dt
  \\
  \simeq \
  \frac{z}{2} \cdot \left [ C_1 \cdot f\left(\frac{z}{2} \cdot t_1 + \frac{z}{2}\right)
                            + ...
                            + C_n \cdot f\left(\frac{z}{2} \cdot t_n + \frac{z}{2}\right)
                    \right ]
  \\
  = \
  \frac{z}{2} \cdot \sum_{i=1}^{n}{C_i \cdot f\left(\frac{z}{2} \cdot t_i + \frac{z}{2}\right)}
\end{array}
\]

조금 더 복잡해 보일 수 있지만 *z*와 관련된 분수는 고정 숫자이므로 합과 *f(t)* 값의 평가는 여전히 꽤 간단합니다.

그렇다면 이 계산을 수행하려면 무엇이 필요할까요? 우선 *f(t)*에 대한 명시적 공식이 필요합니다. 도함수 표기법은 종이에서는 편리하지만 구현해야 할 때는 그렇지 않기 때문입니다. 또한 이러한 *C<sub>i</sub>* 및 *t<sub>i</sub>* 값이 무엇이어야 하는지 알아야 합니다. 다행히도 실제로 모든 *n*에 대해 이러한 값을 제공하는 많은 테이블이 있으므로 작업이 덜 합니다. 두 항으로만 적분을 근사하려는 경우(실제로는 약간 낮음) [이러한 테이블](./legendre-gauss.html)은 *n=2*의 경우 다음 값을 사용해야 한다고 알려줍니다.

\[
\begin{array}{l}
C_1 = 1 \\
C_2 = 1 \\
t_1 = - \frac{1}{\sqrt{3}} \\
t_2 = + \frac{1}{\sqrt{3}}
\end{array}
\]

즉, 적분을 근사하려면 이 값을 근사 함수에 대입해야 하며, 다음을 얻습니다.

\[
\int_{0}^{z}\sqrt{ \left (dx/dt \right )^2+\left (dy/dt \right )^2} dt
≃
\frac{z}{2} \cdot \left [ f\left( \frac{z}{2} \cdot \frac{-1}{\sqrt{3}} + \frac{z}{2} \right)
              + f\left( \frac{z}{2} \cdot \frac{1}{\sqrt{3}} + \frac{z}{2} \right)
          \right ]
\]

*f(t)*를 사용할 수 있다면 상당히 쉽게 프로그래밍할 수 있으며, 베지에 곡선 함수 B<sub>x</sub>(t) 및 B<sub>y</sub>(t)에 대한 전체 설명을 알고 있으므로 가지고 있습니다.

</div>

*C* 값(각 스트립의 두께)과 *t* 값(각 스트립의 위치)에 Legendre-Gauss 값을 사용하면 Legendre-Gauss 합을 계산하여 베지에 곡선의 근사 길이를 결정할 수 있습니다. 다음 그래픽은 계산된 길이와 함께 3차 곡선을 보여줍니다. 곡선을 변경하여 길이가 어떻게 변하는지 확인해보세요. 시도해볼 가치가 있는 한 가지는 직선을 만들 수 있는지 확인하고 길이가 예상과 일치하는지 확인하는 것입니다. 제어점이 바깥쪽에 있고 시작/끝 점이 안쪽에 있는 선을 만들면 어떻게 될까요?

<graphics-element title="베지에 곡선의 호 길이" src="./arclength.js"></graphics-element>
