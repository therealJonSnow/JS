<template>
    <div class="flex flex-col w-full p-4 min-h-screen">
      <div class="flex items-center space-x-4">
        <!-- fixed top-0 left-16  -->
        <TheLogo @click="$router.go('/')" class="w-24 h-fit"/>
        <div ref="titleBlock" class="relative overflow-hidden">
          <h1 class="text-5xl mt-4 opacity-0">//Blog</h1>
          <div class="absolute top-0 left-0 w-0 h-full bg-gray-800"></div>
        </div>
      </div>
      <div class="box" />
      
      
      <TheSpinner />
      <main class="mx-auto text-end w-full max-w-4xl space-y-6">
        <ul class="">
          <li v-for="post of posts" :key="post.slug">
            <!-- <NuxtLink :to="post._path" :data-before="post.title">{{ post.title }}</NuxtLink> -->
            <NuxtLink to="/blog" :data-before="post.title">{{ post.title }}
            </NuxtLink>
            <svg viewBox="0 0 100 100">
    <mask id="myMask"></mask>

    <image class="image" x=0 y=0 width="100%" height="100%" href="https://assets.codepen.io/489403/grand_canyon.jpeg" preserveAspectRatio="xMidYMid slice" mask="url(#myMask)" />
  </svg>
          </li>
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