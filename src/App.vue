<script setup>
import { onMounted, ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const drawerOpen = ref(false)
const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const cart = ref([])
const items = ref([])
const sortBy = ref('teamName')
const searchQuery = ref('')

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
  console.log(cart)
}

const onChangeSelect = (event) => {
  sortBy.value = event.target.value
}

const onChangeSearchInput = (event) => {
  searchQuery.value = event.target.value
}

const fetchItems = async () => {
  try {
    let modifiedSearchQuery = searchQuery.value
    if (searchQuery.value) {
      modifiedSearchQuery = `*${searchQuery.value}*`
    }

    await axios
      .get(
        `https://200b4cae67943554.mokky.dev/items?teamName=*${modifiedSearchQuery}&sortBy=${sortBy.value}`
      )
      .then((response) => {
        items.value = response.data
      })
      .catch((error) => {
        console.log(error)
      })
  } catch (error) {
    console.log(error)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch([sortBy, searchQuery], () => {
  fetchItems()
})

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', {
  cart,
  addToCart,
  removeFromCart,
  closeDrawer,
  openDrawer
})
</script>

<template>
  <Drawer v-if="drawerOpen" :total-price="totalPrice" />
  <div class="w-4/5 m-auto">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />

    <div p-10 class="mt-5">
      <div class="flex justify-between items-center">
        <h2 class="text-2xl font-bold">Бейсболки</h2>

        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="teamName">По команде</option>
            <option value="price">По цене (дешевле)</option>
            <option value="-price">По цене (дороже)</option>
          </select>

          <div class="relative">
            <img class="absolute top-3 left-4" src="/search.svg" />
            <input
              @input="onChangeSearchInput"
              class="border rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>

      <CardList class="mt-5" :items="items" @add-to-cart="onClickAddPlus" />
    </div>
  </div>
</template>

<style scoped></style>
