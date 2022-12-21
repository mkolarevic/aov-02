<template>
  <div class="w-full h-full flex justify-center items-center flex-col">
    <template v-if="joke">
      <p>{{ joke.setup }}</p>
      <p v-if="state === 'step-2'">{{ joke.delivery }}</p>
      <button
        v-if="state === 'step-1'"
        role="button"
        aria-label="Tell me!"
        name="Tell me!"
        @click="showPunchline"
        class="px-4 py-2 font-semibold text-sm bg-red-500 text-white rounded-full shadow-sm mt-4"
      >
        Tell me!
      </button>
      <button
        v-if="state === 'step-2'"
        role="button"
        aria-label="Another"
        name="Another"
        @click="getJoke"
        class="px-4 py-2 font-semibold text-sm bg-red-500 text-white rounded-full shadow-sm mt-8"
      >
        Another
      </button>
    </template>
    <button
      v-if="state === 'error'"
      role="button"
      aria-label="Error"
      @click="getJoke"
      class="px-4 py-2 font-semibold text-sm bg-red-500 text-white rounded-full shadow-sm mt-8"
    >
      Refresh
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
const joke = ref(null)
const state = ref('init')

const showPunchline = () => {
  state.value = 'step-2'
}

onMounted(() => {
  getJoke()
})

const getJoke = () => {
  state.value = 'loading'
  fetch('https://v2.jokeapi.dev/joke/christmas')
    .then(res => res.json())
    .then(value => {
      joke.value = value
    })
    .catch(() => {
      alert('There was an error')
    })
    .finally(() => {
      state.value = 'step-1'
      console.log(state.value)
      console.log(joke.value)
    })
}
</script>
