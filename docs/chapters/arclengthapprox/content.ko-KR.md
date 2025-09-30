# 근사 호 길이

때로는 실제로 진정한 호 길이의 정밀도가 필요하지 않고, 대신 근사 호 길이를 계산하는 것만으로 충분한 경우가 있습니다. 이를 수행하는 가장 빠른 방법은 곡선을 평탄화한 다음 단순히 점에서 점까지의 선형 거리를 계산하는 것입니다. 이것은 오차가 있지만 세그먼트 수를 늘려 임의로 작게 만들 수 있습니다.

이전 섹션에서 곡선 평탄화와 호 길이 계산에 대해 수행한 작업을 결합하면 최소한의 노력으로 구현할 수 있습니다.

<div class="figure">

<graphics-element title="2차 곡선 근사 호 길이" src="./approximate.js" data-type="quadratic">
    <input type="range" min="2" max="24" step="1" value="4" class="slide-control">
</graphics-element>

<graphics-element title="3차 곡선 근사 호 길이" src="./approximate.js" data-type="cubic">
    <input type="range" min="2" max="32" step="1" value="8" class="slide-control">
</graphics-element>

</div>

절대값으로는 길이 오차가 실제로 꽤 큰데도, 적은 수의 세그먼트에서도 호 길이의 정수 부분만 봤을 때는 진정한 길이와 일치하는 길이를 얻는다는 것을 알 수 있습니다. 근사는 종종 작업을 엄청나게 빠르게 만들 수 있습니다!
