<template>
  <div id="cursor" class="pointer-events-none">
      <div class="circle circle-cyan" />
      <div class="circle circle-red" />
    </div>
</template>

<script setup>
onMounted(() => {
  const cursorRounded = document.querySelector("#cursor");
  
  const moveCursor = (e) => {
    const mouseY = e.clientY;
    const mouseX = e.clientX;
  
    cursorRounded.style.transform = `translate3d(calc(${mouseX}px - 1rem), calc(${mouseY}px - 1rem), 0)`;
    if (e.target.nodeName === 'A') {
      cursorRounded.classList.add("hovered");
    } else {
      cursorRounded.classList.remove("hovered");
    }
  };
  
  window.addEventListener("mousemove", moveCursor);
})
</script>
<style>
#cursor {
  position: fixed;
  top: 0;
  left: 0;
  transition: opacity 0.3s ease;
  opacity: 0;
}
body:hover #cursor {
  opacity: 1;
}
.circle {
  width: 2rem;
  height: 2rem;
  border-radius: 100%;
  top: 0;
  position: absolute;
  left: 0;
  mix-blend-mode: multiply;
  pointer-events: none;
  border: 4px solid transparent;
  transition: all 0.3s ease;
}
.circle-red {
  translate: -1px -1px;
  border-color: var(--primary);
}
#cursor.hovered .circle-red {
  translate: 20% 20%;
  background-color: var(--primary);
  border-width: 0;
  opacity: 0.9;
}
.circle-cyan {
  translate: 1px 1px;
  border-color: var(--secondary);
}
#cursor.hovered .circle-cyan {
  translate: -20% -20%;
  background-color: var(--secondary);
  border-width: 0;
  opacity: 0.9;
}
</style>