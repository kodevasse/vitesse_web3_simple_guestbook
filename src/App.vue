<script setup lang="ts">
// https://github.com/vueuse/head
// you can use this to manipulate the document head in any components,
// they will be rendered correctly in the html results with vite-ssg
useHead({
  title: 'PDPkrypto\'s Guest Book',
  meta: [
    { name: 'Guest Book @ Polygon Mumbai', content: 'PDPkrypto crypto project' },
    { property: 'og:image', content: 'https://lh3.googleusercontent.com/T__nKUzSj5swjXGiuh1Mx2UtaojaKouwLRzZ24auTBns0VWwWJfLzWnW6f1KEMC0RLQbcsmy_nQbnrteXtQ1MpD8PZ2Epuw-ML8Q=s0' },
  ],
})
</script>

<template>
  <router-view />
</template>
