<template>
  <div class="w-full h-full flex justify-center items-center">
    <div v-if="joke" class="max-w-xs w-full flex flex-col">
      <div class="w-3/4 p-4 rounded-2xl bg-teal-800 text-white self-start">{{ joke.setup }}</div>
      <div v-if="deliveryShown" class="w-3/4 mt-2 p-4 rounded-2xl bg-red-800 text-white self-end">
        {{ joke.delivery }}
      </div>
      <button @click="handleClick" class="bg-green col-span-1 rounded-lg py-2 hover:opacity-90 w-full mx-auto mt-4">
        {{ deliveryShown ? 'Another' : 'Tell Me!' }}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

type Joke = {
  setup: string
  delivery: string
}

const joke = ref<Joke | null>(null)
const deliveryShown = ref(false)

const getJoke = async () => {
  const response = await fetch('https://v2.jokeapi.dev/joke/christmas')
  joke.value = await response.json()
}

const handleClick = async () => {
  if (deliveryShown.value) {
    joke.value = null
    await getJoke()
  }
  deliveryShown.value = !deliveryShown.value
}

getJoke()
</script>
