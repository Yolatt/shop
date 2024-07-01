<script setup>
defineProps({
  title: String,
  teamName: String,
  imageUrl: String,
  price: Number,
  isAdded: Boolean,
  onClickAdd: Function
})
</script>

<template>
  <div
    class="relative border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl tansition"
  >
    <img :src="imageUrl" alt="Cap" />

    <p>{{ teamName }}</p>
    <b class="mt-2">{{ title }}</b>
    <div class="flex justify-between mt-5">
      <b>{{ price }} ₽</b>
      <img
        v-if="onClickAdd"
        @click="onClickAdd"
        :src="!isAdded ? '/plus.svg' : 'checked.svg'"
        alt="Add to cart"
      />
    </div>
  </div>
</template>
