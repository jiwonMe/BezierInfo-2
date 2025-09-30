# 바운딩 박스

극값과 시작/끝 점이 있으면, x와 y의 최솟값/최댓값을 테스트하는 간단한 for 루프를 사용하여 곡선을 감싸는 데 필요한 네 가지 값을 얻을 수 있습니다.

*베지에 곡선의 바운딩 박스 계산*:

1. 곡선 도함수의 x 및 y 근에 대한 모든 *t* 값을 찾습니다.
2. 0보다 낮거나 1보다 높은 *t* 값은 버립니다. 베지에 곡선은 [0,1] 구간만 사용하기 때문입니다.
3. *t=0*, *t=1* 및 찾은 각 근을 원래 함수에 대입했을 때 가장 낮은 값과 가장 높은 값을 결정합니다. 가장 낮은 값이 하한이고 가장 높은 값이 구성하려는 바운딩 박스의 상한입니다.

이 접근 방식을 이전의 근 찾기에 적용하면 다음과 같은 [축 정렬 바운딩 박스](https://en.wikipedia.org/wiki/Bounding_volume#Common_types)를 얻습니다(모든 곡선 극값 점이 곡선에 표시됨):

<div class="figure">
<graphics-element title="2차 베지에 바운딩 박스" src="./bbox.js" data-type="quadratic"></graphics-element>
<graphics-element title="3차 베지에 바운딩 박스" src="./bbox.js" data-type="cubic"></graphics-element>
</div>

x축과 y축을 따라 정렬하는 대신 곡선을 따라 정렬하여 더 멋진 박스를 구성할 수도 있지만, 그렇게 하려면 먼저 정렬이 어떻게 작동하는지 살펴봐야 합니다.
