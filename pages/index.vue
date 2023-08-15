<template>
  <div class="grid grid-cols-[1fr_min-content_1fr] w-full h-full justify-center items-center">
    <NuxtLink to="/about" data-before="- About -">- About -
    </NuxtLink>
    <TheLogo />
    <NuxtLink to="/blog" data-before="- Blog -">- Blog -
    </NuxtLink>
    <TheSpinner />
  </div>
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