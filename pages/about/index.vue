<template>
  <div class="flex flex-col w-full">
    <div class="flex items-center space-x-4 p-12 pb-0">
      <!-- fixed top-0 left-16  -->
      <NuxtLink to="/" class="w-fit m-0 mb-4">
        <TheLogo class="w-24 h-fit"/>
      </NuxtLink>
      <div ref="titleBlock" class="relative overflow-hidden">
        <h1 class="title text-5xl mt-4 opacity-0">//About</h1>
        <div class="absolute top-0 left-0 w-0 h-full bg-gray-800"></div>
      </div>
    </div>
  
    <TheSpinner />
    <main class="mx-auto prose lg:prose-xl xl:prose-2xl text-gray-800 overflow-auto">
      <p>
        Hi there! I'm Jonny, a <span class="span-highlight">Front-end Developer</span> and creative based out of Bristol, UK.
      </p>
      <p>
        In my spare time I like making <span class="span-highlight">things.</span> Creating or developing my skills to bring ideas to life. Adding those extra touches that make peoples heads turn and take notice.
      </p>
      <p />

      <h6 class="title ml-8 !mb-0">
        in emergency: <span class="span-highlight">add saltwater </span>
      </h6>
      <h2 class="title !mt-0">The_<wbr>Essentials</h2>
      <ul class="mb-12">
          <li>Water. Preferably in large amounts. Doesn't matter if its frozen as long as I'm up a mountain.</li>
          <li>Boardsports for sanity: windsurfing, skating, snowboarding, surfing, kitesurfing ...</li>
          <li>Campervan for those weekend trips; boards don't carry themselves</li>
          <li>Peanut butter.</li>
      </ul>

      <h6 class="title ml-8 !mb-0">
        a <span class="span-highlight">mixed bag </span>
      </h6>
      <h2 class="title !mt-0">My_<wbr>Skillset</h2>
      <p class="">
        I am a strong communicator who enjoys working with diverse clients through a pragmatic and flexible process to deliver beautiful optimised digital solutions.
      </p>
      <p class="">
        As a marine engineer turned developer I'm not your usual suspect, but if you need a website built underwater I'm your guy.
      </p>
      <p />
      <ul class="mb-12">
          <li>Strong HTML, CSS and JavaScript</li>
          <li>Experience with Vue and Nuxt.js</li>
          <li>A love of css animations and ui transitions</li>
          <li>Knowledge of Git, GitFlow, CI & CD</li>
          <li>MEng in Marine Engineering from Southampton University</li>
      </ul>
    </main>

  </div>
</template>

<script setup>
import gsap from 'gsap'
const titleBlock = ref(null)
onMounted(() => {
gsap.timeline({delay: 0.5})
  .to(titleBlock.value.children[1], { width: '100%', duration: 0.4 })
  .set(titleBlock.value.children[0], { opacity: 1})
  .to(titleBlock.value.children[1], { left: '100%', duration: 0.4 })
})
const { data: posts } = await useAsyncData('posts', () => {
return queryContent('/blog').find()
})
</script>