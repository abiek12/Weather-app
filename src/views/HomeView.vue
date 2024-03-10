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
      <ul
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
        v-if="mapboxSearchResults"
      >
        <p class="py-2" v-if="searchError">Sorry, something went wrong, please try again.</p>
        <p class="py-2" v-if="!serverError && mapboxSearchResults.length === 0">
          No results match your query, try a different term.
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="py-2 cursor-pointer"
            @click="previewCity(searchResult)"
          >
            {{ searchResult.name }}
          </li>
        </template>
      </ul>
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
      try {
        const response = await axios.get(
          `https://api.openweathermap.org/geo/1.0/direct?q=${searchQuery.value}&limit=5&appid=${mapboxAPIKey}`
        )
        if (response.data && response.data.length > 0) {
          mapboxSearchResults.value = response.data.map((city) => ({
            id: city.name,
            name: `${city.name}, ${city.state}, ${city.country}`
          }))
        } else {
          mapboxSearchResults.value = []
        }
      } catch (error) {
        console.error('Error fetching data:', error)
        mapboxSearchResults.value = []
      }
    } else {
      mapboxSearchResults.value = null
    }
  }, 300)
}
</script>
