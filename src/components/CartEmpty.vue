<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex flex-col items-center text-center w-72 mx-auto">
    <img height="100" width="100" :src="imageUrl" alt="Cart info image" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p>{{ description }}</p>
  </div>
</template>
