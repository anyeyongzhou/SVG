<template>
  <div class="container">
    <div class="container-box" data-num="--" id="box">
      <div class="circle-outer"></div>
      <svg>
        <defs>
          <radialGradient
            id="gradient"
            cx="50%"
            cy="50%"
            r="60%"
            fx="50%"
            fy="50%"
          >
            <stop offset="30%" stop-color="var(--ring-color2)" />
            <stop offset="100%" stop-color="var(--ring-color1)" />
          </radialGradient>
        </defs>
        <circle stroke="url(#gradient)" id="circle"></circle>
      </svg>
    </div>
  </div>
</template>

<script script setup lang="ts">
import { onMounted } from 'vue'
const getData = () => {
  const circle = document.getElementById('circle')
  const box = document.getElementById('box')
  let perimeter = 3.14 * 150 //这是stroke-dasharray的值
  let i = 0
  let timer: any = null
  const loading = () => {
    if (i < 100) {
      i++
      box!.dataset.num = i + '%'
      let per = perimeter - (perimeter * i) / 100
      circle!.style.strokeDashoffset = per.toString()
    } else {
      clearInterval(timer)
    }
  }
  loading()
  timer = setInterval(loading, 100)
}
onMounted(() => {
  getData()
})
</script>
<style lang="less" scoped>
body {
  background: var(--back-bg);
}
:root {
  --bg: #edf1f5;
  --back-bg: #c5ccda;
  --back-shadow: #ffffff;
  --ring-color1: #c1dfc4;
  --ring-color2: #3cba92;
  --text-aolor: #497241;
  /* 父容器宽度 */
  --w: 200px;
  /* 父容器gap*/
  --gap: 30px;
  /* 第二层圆形宽度 */
  --inner: calc(var(--w) - var(--gap));
  /* 环形stroke宽度 */
  --stroke: 20px;
  /* svg环形宽度 */
  --circle: calc(var(--inner) - var(--stroke));
  /* 第三层圆形宽度 */
  --center: calc(var(--circle) - var(--stroke));
}
.container {
  padding: 30px;
  border-radius: 20px;
  background: var(--bg);
  box-shadow: 0px 20px 30px rgba(100, 131, 177, 0.2);
  border: 1px solid #fff;
}
.container-box {
  position: relative;
  width: var(--w);
  height: var(--w);
}
.circle-outer {
  position: relative;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  box-shadow: 6px 6px 8px var(--back-bg), -6px -6px 8px var(--back-shadow);
  background: var(--bg);
}
.circle-outer::before {
  content: '';
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  background: var(--bg);
  width: var(--inner);
  height: var(--inner);
  box-shadow: inset 8px 8px 10px var(--back-bg),
    inset -4px -4px 8px var(--back-shadow);
}
.circle-outer::after {
  content: '';
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  background: var(--bg);
  width: var(--center);
  height: var(--center);
  box-shadow: 6px 6px 8px var(--back-bg), -2px -2px 8px var(--back-shadow);
}
.container-box::after {
  content: attr(data-num);
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  color: var(--text-aolor);
  font-size: 30px;
}
svg {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  transform: rotate(-90deg);
}
svg circle {
  position: absolute;
  --z: calc(var(--w) / 2);
  --c: calc(var(--circle) / 2);
  transform: translate(calc(var(--z) - var(--c)), calc(var(--z) - var(--c)));
  cy: calc(var(--circle) / 2);
  cx: calc(var(--circle) / 2);
  r: calc(var(--circle) / 2);
  fill: none;
  stroke-linecap: round;
  /* 设置圆的stroke-dasharray和stroke-dashoffset，为圆的周长 */
  stroke-dasharray: calc(3.14 * var(--circle));
  stroke-dashoffset: calc(3.14 * var(--circle));
  stroke-width: var(--stroke);
}
</style>
