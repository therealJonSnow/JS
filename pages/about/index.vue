<template>
  <div class="flex flex-col w-full p-4 min-h-screen">
    <div class="flex items-center space-x-4">
      <!-- fixed top-0 left-16  -->
      <TheLogo @click="$router.go('/')" class="w-24 h-fit"/>
      <div ref="titleBlock" class="relative overflow-hidden">
        <h1 class="text-5xl mt-4 opacity-0">//About</h1>
        <div class="absolute top-0 left-0 w-0 h-full bg-gray-800"></div>
      </div>
    </div>
    <div class="box" />  
  
    <TheSpinner />
    <main class="mx-auto text-end w-full max-w-4xl space-y-6">
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