<script setup>
import axios from 'axios'
import { ref, computed, inject } from 'vue'

import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import CartEmpty from './CartEmpty.vue'

const props = defineProps({
  totalPrice: Number
})

const { cart, closeDrawer } = inject('cart')

const isCreatingOrder = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://200b4cae67943554.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = []

    orderId.value = data.id
  } catch (error) {
    console.log(error)
  } finally {
    isCreatingOrder.value = false
  }
}

const buttonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)
const cartIsEmpty = computed(() => cart.value.length === 0)
</script>
<template>
  <div>
    <div class="fixed top-0 left-0 h-full w-full bg-black opacity-50 z-10"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
      <DrawerHead />

      <div v-if="!totalPrice || orderId" class="flex h-full items-center">
        <CartEmpty
          v-if="!totalPrice && !orderId"
          title="Свышь купи"
          description="Ваша корзина пуста, Тодд не одобряет"
          image-url="/package-icon.png"
        />
        <CartEmpty
          v-if="orderId"
          title="Заказ оформлен"
          :description="`Ваш заказ № ${orderId} оформлен и будет передан курьерской службе в ближайшее время`"
          image-url="/order-success-icon.png"
        />
      </div>

      <div v-else>
        <CartItemList />

        <div class="flex flex-col gap-2 mt-7">
          <div>
            <span class="mr-2">Итого: </span>
            <b>{{ totalPrice }} ₽</b>
          </div>
          <div class="flex-1 border-b border-dashed"></div>

          <button
            :disabled="buttonDisabled"
            @click="createOrder"
            class="mt-3 transition bg-purple-500 w-full rounded-xl py-3 text-white antialiased disabled:bg-slate-400 hover:bg-purple-600 active:bg-purple-700 cursor-pointer"
          >
            Оформить заказ
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
