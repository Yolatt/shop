<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  title: String,
  teamName: String,
  imageUrl: String,
  price: Number
})
</script>

<template>
  <div class="flex items-center border border-teal-100 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="imageUrl" :alt="teamName" />

    <div class="flex flex-col justify-between">
      <p>{{ teamName }}</p>
      <b>{{ title }}</b>
      <div class="flex justify-between mt-2">
        <b>{{ price }} ₽</b>
        <img
          @click="emit('onClickRemove')"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
          alt="close"
        />
      </div>
    </div>
  </div>
</template>
