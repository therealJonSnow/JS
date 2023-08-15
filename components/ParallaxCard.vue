<template>
  <div
    ref="page"
    class="page"
    @mousemove="(e) => onMouseMoveHandler(e)"
    @mouseenter="(e) => onMouseEnterHandler(e)"
  >
    <div
      class="wrapper opacity-0"
      ref="wrapper"
    >
      <slot />
    </div>
  </div>
</template>

<script setup>
import gsap from 'gsap'
import { Elastic } from 'gsap';

const page = ref(null)
const wrapper = ref(null)

let mouse = ref(null)
let counter = ref(0);
let updateRate = ref(1);
let perspectiveEnabled = ref(true)

onMounted(() => {
  setTimeout(() => {
  mouse.value = {
    _x: 0,
    _y: 0,
    x: 0,
    y: 0,
    updatePosition: function (event) {
      var e = event || window.event;
      this.x = e.clientX - this._x;
      this.y = (e.clientY - this._y) * -1;
    },
    setOrigin: function (e) {
      this._x = e.offsetLeft + Math.floor(e.offsetWidth / 2);
      this._y = e.offsetTop + Math.floor(e.offsetHeight / 2);
    },
    show: function () {
      return "(" + this.x + ", " + this.y + ")";
    }
  };
  mouse.value.setOrigin(wrapper.value);
  counter.value = 0;
  updateRate.value = 1;
  console.log('init', page.value, wrapper.value)

  gsap.fromTo(wrapper.value, { z:-30}, {duration: 2, z: 0, ease: Elastic.easeOut.config(1, 0.7)})
  gsap.fromTo(wrapper.value, { opacity: 0}, {duration: 0.7, opacity: 1})
  }, 1)
})

function isTimeToUpdate() {
  return counter.value++ % updateRate.value === 0;
}
function updateBoxStyle(x, y) {
  if (perspectiveEnabled) {
    gsap.set(wrapper.value, { rotateX: x+'deg', rotateY: y+'deg'})
  }
}
function update(event) {
  mouse.value.updatePosition(event);
  updateBoxStyle(
    (
      -(event.clientY - mouse.value._y) /
      wrapper.value.offsetHeight /
      2
    ).toFixed(2),
    (
      (event.clientX - mouse.value._x) /
      wrapper.value.offsetWidth /
      2
    ).toFixed(2)
  );
}
function onMouseEnterHandler(event) {
  update(event);
}

function onMouseMoveHandler(event) {
  if (isTimeToUpdate()) {
    update(event);
  }
}





</script>

<style>
.page {
  perspective: 100px;
  overflow: hidden;
  position: relative;
  width: 100%;
  cursor: none;
  height: 100%;
}

.wrapper {
  overflow: hidden;
  position: relative;
  width: 100%;
  cursor: none;
  height: 100%;
  padding: 4rem;
}
</style>