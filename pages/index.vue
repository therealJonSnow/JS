<template>
  <ParallaxCard>
    <div class="grid grid-cols-[1fr_min-content_1fr] grow w-full h-full justify-center items-center">
      <NuxtLink to="/about" class="title" data-before="- About -">- About -
      </NuxtLink>
      <TheLogo />
      <NuxtLink to="/blog" class="title" data-before="- Blog -">- Blog -
      </NuxtLink>
      <TheSpinner />
    </div>
    <div class="box">
    </div>
  </ParallaxCard>
</template>

<script setup>
definePageMeta({
  pageTransition: {
    name: 'test',
    mode: 'out-in',
    onBeforeLeave: () => {
      console.log('leave')
    }
  }
})
useHead({
  title: 'Home',
  script: [
    { src: "https://identity.netlify.com/v1/netlify-identity-widget.js" },
  ]
})
</script>