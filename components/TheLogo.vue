<template>
  <svg @mouseenter="mouseEnter" @mouseleave="mouseLeave" viewBox="-40 0 450 708" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path
      ref="red"
      d="M15.7729 624.025C58.4275 697.905 152.897 723.218 226.777 680.564C300.657 637.909 325.971 543.439 283.316 469.559L17.3298 8.85791M381.045 85.291C338.391 11.411 243.92 -13.9021 170.041 28.7526C96.1606 71.4072 70.8474 165.877 113.502 239.757L302.79 567.616"
      stroke="#FE0000"
      stroke-width="15"
    />
    <g style="mix-blend-mode: multiply">
      <path
        ref="blue"
        d="M5.86377 623.367C48.5184 697.247 142.988 722.56 216.868 679.906C290.748 637.251 316.061 542.781 273.407 468.901L7.42064 8.1997M371.136 84.6328C328.481 10.7528 234.011 -14.5603 160.131 28.0944C86.2514 70.749 60.9382 165.219 103.593 239.099L292.881 566.958"
        stroke="#00FFE0"
        stroke-width="15"
      />
    </g>
  </svg>
</template>

<script setup>
import gsap from 'gsap'
import DrawSVGPlugin from 'gsap/DrawSVGPlugin'
import { Elastic, Power2 } from 'gsap';
gsap.registerPlugin(DrawSVGPlugin)

const red = ref(null)
const blue = ref(null)
onMounted(() => {
  setTimeout(() => {
    
    gsap.set(red.value, { x: '-6px', y: '-0.5px'})
    gsap.set(blue.value, { x: '6px', y: '-0.5px'})
    gsap.fromTo([blue.value, red.value], {drawSVG:"50% 50%"}, {duration: 1, drawSVG:"100%", stagger: 0.1, ease: Power2.easeInOut})
    gsap.to([red.value, blue.value], { x: '0px', y: '0px', duration: 2, delay: 1, ease: Elastic.easeOut.config(2.5, 0.3)})
  }, 1);
})

function mouseEnter() {
  gsap.to(red.value, { x: '20px', y: '5px', duration: 1})
  gsap.to(blue.value, { x: '-20px', y: '-5px', duration: 1})
}
function mouseLeave() {
  gsap.to([red.value, blue.value], { x: '0px', y: '0px', duration: 2, ease: Elastic.easeOut.config(2.5, 0.3)})

}
</script>

<style>
@media only screen and (max-width: 1000px) {
  .wrapper {
    grid-template-columns: 1fr;
    grid-template-rows: 1fr min-content min-content;
  }

  svg {
    order: -1;
    margin-bottom: 10vh;
  }
}

svg {
  height: clamp(20rem, calc(-3.02rem + 27.52vw), 30rem);
}

</style>