<template>
  <div v-for="city in savedCities" :key="city.id">
    <CityCard :city="city" @click="goToCityView(city)" />
  </div>

  <p v-if="savedCities.length === 0">
    No locations added. To start tracking a location, search in the field above.
  </p>
</template>

<script setup>
import axios from 'axios'
import { ref } from 'vue'
import CityCard from './CityCard.vue'
import { useRouter } from 'vue-router'

const savedCities = ref([])
const getCities = async () => {
  if (localStorage.getItem('savedCities')) {
    savedCities.value = JSON.parse(localStorage.getItem('savedCities'))
    const requests = []
    savedCities.value.forEach((city) => {
      requests.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lon}&appid=0cc9e70ed1c45d1f75ad22b3365aba0c&units=imperial`
        )
      )
    })
    const waetherData = await Promise.all(requests)
    waetherData.forEach((value, index) => {
      savedCities.value[index].weather = value.data
    })
  }
}
await getCities()
const router = useRouter()
const goToCityView = (cities) => {
  const id = cities.id
  const state = cities.state
  const city = cities.name
  const lat = cities.coords.lat
  const lon = cities.coords.lon
  router.push({
    name: 'cityView',
    params: { state, city },
    query: { id: id, lat: lat, lon: lon }
  })
}
</script>
