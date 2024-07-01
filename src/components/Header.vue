<script setup>
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-teal-500 px-12 py-6">
    <div class="flex items-center">
      <img src="/logo.png" alt="Logo" class="w-10 mr-5" />
      <h2 class="text-xl font-bold upper">RaccoCaps</h2>
    </div>

    <!-- потом переписать корзину -->
    <ul class="flex items-center cursor-pointer gap-10">
      <li @click="() => emit('openDrawer')" class="flex items-center gap-3 hover:text-black">
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }} ₽</b>
      </li>
    </ul>
  </header>
</template>
