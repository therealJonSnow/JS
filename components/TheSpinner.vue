<template>
  <section class="group" @mouseenter="changeRotationSpeed(10)" @mouseleave="changeRotationSpeed(1)">
    <h1 ref="ring" class="text-ring"></h1>
    <div class="text-3xl translate-y-1.5 scale-0 transition-transform duration-1000 group-hover:scale-100 ease-in-out">👋</div>
  </section>
</template>

<script setup>
import gsap from 'gsap'

const ring = ref(null)
const rotateAnim = ref(null)
onMounted(() => {
  const canTrig = CSS.supports("(top: calc(sin(1) * 1px))");
  const HEADING = ring.value

  const OPTIONS = {
    SPACING: 1,
    SIZE: 0.25,
    TEXT: "A Pen by Jon Snow • "
  };

  const onUpdate = () => {
    // Make the ring text
    const text = OPTIONS.TEXT;
    // 1. Take the text and split it into spans...
    const chars = text.split("");
    HEADING.innerHTML = "";
    HEADING.style.setProperty("--char-count", chars.length);

    for (let c = 0; c < chars.length; c++) {
      HEADING.innerHTML += `<span aria-hidden="true" class="char" style="--char-index: ${c};">${chars[c]}</span>`;
    }
    HEADING.innerHTML += `<span class="sr-only">${OPTIONS.TEXT}</span>`;
    // Set the styles
    HEADING.style.setProperty("--font-size", OPTIONS.SIZE);
    HEADING.style.setProperty("--character-width", OPTIONS.SPACING);
    HEADING.style.setProperty(
      "--radius",
      canTrig
        ? "calc((var(--character-width) / sin(var(--inner-angle))) * -1ch"
        : `calc(
        (${OPTIONS.SPACING} / ${Math.sin(
            360 / HEADING.children.length / (180 / Math.PI)
          )})
        * -1ch
      )`
    );
  };

  rotateAnim.value = gsap.to(ring.value, {
    rotation: 360,
    ease: "none",
    duration: 20,
    repeat: -1
  })

  onUpdate();

})

function changeRotationSpeed (speed) {
  gsap.to(rotateAnim.value, { timeScale: speed, duration: 1 });
}
</script>

<style>
:root {
  --gray-0: #f8f9fa;
  --gray-1: #f1f3f5;
  --gray-2: #e9ecef;
  --gray-3: #dee2e6;
  --gray-4: #ced4da;
  --gray-5: #adb5bd;
  --gray-6: #868e96;
  --gray-7: #495057;
  --gray-8: #343a40;
  --gray-9: #212529;
  --gray-10: #16191d;
  --gray-11: #0d0f12;
  --gray-12: #030507;
  /* Surfaces */
  --text-1: var(--gray-12);
  --text-2: var(--gray-10);
  --text-3: var(--gray-8);
  --text-4: var(--gray-7);
  --surface-1: var(--gray-0);
  --surface-2: var(--gray-1);
  --surface-3: var(--gray-2);
  --surface-4: var(--gray-3);
}

@media (prefers-color-scheme: dark) {
  :root {
    --text-1: var(--gray-1);
    --text-2: var(--gray-3);
    --text-3: var(--gray-5);
    --text-4: var(--gray-6);
    --surface-1: var(--gray-10);
    --surface-2: var(--gray-9);
    --surface-3: var(--gray-8);
    --surface-4: var(--gray-7);
  }
}

*,
*:after,
*:before {
  box-sizing: border-box;
}

.sr-only {
  /* font-family: 'Google Sans', sans-serif, system-ui; */
  position: absolute;
  z-index: 50;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

/*
 * To get the radius or the hypoteneuse.
 * If you know the inner angle and the length of the side
 * The end / Math.sin(innerAngle) should provide the radius
*/

section:first-of-type {
  aspect-ratio: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  place-items: center;
  position: fixed;
  bottom: 3rem;
  right: 3rem;
  width: 6rem;
}

.text-ring {
  --inner-angle: calc((360 / var(--char-count)) * 1deg);
  --character-width: 1;
  font-family: monospace;
  text-transform: uppercase;
  font-size: calc(var(--font-size, 1) * 10vmin);
  position: absolute;
  box-shadow: none;
}

/* @media (prefers-reduced-motion: no-preference) {
  .text-ring {
    animation: rotation 6s infinite linear;
  }
  section:first-of-type:hover .text-ring {
    animation: rotation 3s infinite linear;
  }
} */

/* @keyframes rotation {
  to {
    rotate: -360deg;
  }
} */

.char {
  display: inline-block;
  position: absolute;
  font-weight: 300;
  top: 50%;
  left: 50%;
  transform:
    translate(-50%, -50%) rotate(calc(var(--inner-angle) * var(--char-index))) translateY(var(--radius));
}
</style>