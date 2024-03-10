<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
    </div>
  </main>
</template>

<script setup>
import axios from 'axios'
import { ref } from 'vue'

const mapboxAPIKey = '0cc9e70ed1c45d1f75ad22b3365aba0c'
const searchQuery = ref('')
const queryTimeOut = ref(null)
const mapboxSearchResults = ref(null)

const getSearchResults = () => {
  clearTimeout(queryTimeOut.value)
  queryTimeOut.value = setTimeout(async () => {
    if (searchQuery.value !== '') {
      const result = await axios.get(
        `http://api.openweathermap.org/geo/1.0/direct?q=${searchQuery.value}&limit=5&appid=${mapboxAPIKey}`
      )
      mapboxSearchResults.value = result.data.features
      console.log(mapboxSearchResults)
      return
    }
    mapboxSearchResults.value = null
  }, 300)
}
</script>
