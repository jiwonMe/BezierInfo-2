# 베지에 경로 그리기

- 마우스, 스타일러스, 손가락으로 그리기
- RDP로 경로를 따라 점의 수 줄이기
- 점들을 통과하는 추상 곡선:
  - 고차 베지에, 분할 및 축소
  - 복합 베지에 맞추기
  - catmull-rom

<div class="figure">
  <Graphic title="베지에 곡선 맞추기" setup={this.setup} draw={this.draw} onClick={this.onClick}>
    <button onClick={this.toggle}>toggle</button>
    <button onClick={this.reset}>reset</button>
    <SliderSet ref={ set => (this.sliders=set) } onChange={this.processTimeUpdate} />
  </Graphic>
</div>
