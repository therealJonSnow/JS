<template>
  <div class="relative h-screen w-screen overflow-y-auto overflow-x-hidden">
    <NuxtPage :key="$route.fullPath"/>
    <TheDots dot-color="blue" class="translate-x-[0.5px] translate-y-[0.5px] mix-blend-multiply" />
    <TheDots dot-color="red" class="-translate-x-[0.5px]  -translate-y-[0.5px] mix-blend-multiply"/>
  </div>
  <TheCursor />
</template>

<script setup>
useHead({
  title: 'NDT Stack',
  meta: [
    { name: 'description', content: 'My amazing site.' }
  ],
  htmlAttrs: {
    lang: 'en'
  }
})
</script>
<style>
:root {
  --primary: red;
  --secondary: cyan;
  --step-3: clamp(1.73rem, calc(1.48rem + 1.24vw), 2.44rem);
}
* {
  cursor: none;
  box-sizing: border-box;
}
body {
  font-family: Poppins, Avenir, Helvetica, Arial, sans-serif;
  font-size: 1.5rem;
  height: 100vh;
  max-width: 100vw;
  overflow: hidden;
}
.title {
  font-weight: bold;
  font-family: Padyakke Expanded One, Avenir, Helvetica, Arial, sans-serif;
}
/* link styles */
a {
  margin: 1rem auto;
  position: relative;
  width: fit-content;
  font-size: var(--step-3);
  text-transform: uppercase;
  mix-blend-mode: multiply;
  transition: translate 0.3s ease;
}
a:hover:before {
  translate: 1px 1px;
}
a:hover:after {
  translate: -2px -2px;
}
a:hover {
  color: transparent;
}
a:before {
  top: 0;
  left: 0;
  transition: translate 0.3s ease;
  content: attr(data-before);
  pointer-events: none;
  position: absolute;
  color: var(--secondary);
  mix-blend-mode: multiply;
}
a:after {
  top: 0;
  pointer-events: none;
  left: 0;
  transition: translate 0.3s ease;
  content: attr(data-before);
  position: absolute;
  color: var(--primary);
  mix-blend-mode: multiply;
}

.test-enter-active,
.test-leave-active {
  transition: all 0.4s;
}
.test-enter-from,
.test-leave-to {
  opacity: 0;
}

.box {
  pointer-events: none;
  overflow: hidden;
  position: fixed;
  padding: 1.5rem;
  width: 100vw;
  top: 0;
  left: 0;
  bottom: 0
}
.box:after,
.box:before {
  content: "";
  position: absolute;
  width: calc(100% - 4rem);
  height: calc(100% - 4rem);
  top: 50%;
  left: 50%;
  mix-blend-mode: multiply;
}
.box:after {
  border: 6px solid var(--primary);
  translate: calc(-50% + 1.5px) calc(-50% + 1.5px);
}
.box:before {
  border: 6px solid var(--secondary);
  translate: calc(-50% - 1.5px) calc(-50% - 1.5px);
}
</style>