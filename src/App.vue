<script setup lang="ts">
// https://github.com/vueuse/head
// you can use this to manipulate the document head in any components,
// they will be rendered correctly in the html results with vite-ssg
useHead({
  title: 'PDPkrypto\'s Guest Book',
  meta: [
    { name: 'Guest Book @ Polygon Mumbai', content: 'PDPkrypto crypto project' },
  ],
})
</script>

<template>
  <router-view />
</template>
