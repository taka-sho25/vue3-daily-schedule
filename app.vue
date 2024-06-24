<script setup lang="ts">
useSeoMeta({
  title: 'Nuxt 3 Minimal Starter',
  ogTitle: 'Nuxt 3 Minimal Starter',
  description: 'Nuxt 3 Minimal Starter',
  ogDescription: 'Nuxt 3 Minimal Starter',
});
</script>

<template>
  <Html lang="ja" />
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>

<style>
:root {
  --vds-bg: #84c9fc;
  --vds-active: #366582;
  --vds-hover: #84dafc7a;
  --vds-border: #005a86;
}
</style>
