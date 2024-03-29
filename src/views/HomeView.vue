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
            {{ searchResult.name }}, {{ searchResult.state }},
            {{ searchResult.counrty }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback>
          <CityCardSkelton />
        </template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import CityCardSkelton from '@/components/CityCardSkelton.vue'
import CityList from '@/components/CityList.vue'
import axios from 'axios'
import { ref } from 'vue'
import { useRouter } from 'vue-router'
const router = useRouter()
const previewCity = (searchResult) => {
  const city = searchResult.name
  const state = searchResult.state
  const lat = searchResult.lat
  const lon = searchResult.lon
  router.push({
    name: 'cityview',
    params: { state, city },
    query: {
      lat: lat,
      lon: lon,
      preview: true
    }
  })
}

const weatherAPIKey = '0cc9e70ed1c45d1f75ad22b3365aba0c'
const searchQuery = ref('')
const queryTimeOut = ref(null)
const mapboxSearchResults = ref(null)

const getSearchResults = () => {
  clearTimeout(queryTimeOut.value)
  queryTimeOut.value = setTimeout(async () => {
    if (searchQuery.value !== '') {
      try {
        const response = await axios.get(
          `https://api.openweathermap.org/geo/1.0/direct?q=${searchQuery.value}&limit=5&appid=${weatherAPIKey}`
        )
        if (response.data && response.data.length > 0) {
          mapboxSearchResults.value = response.data.map((data) => ({
            id: data.name,
            name: `${data.name}`,
            state: `${data.state}`,
            counrty: `${data.country}`,
            lat: `${data.lat}`,
            lon: `${data.lon}`
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
