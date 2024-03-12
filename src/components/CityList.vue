<template>
  <div v-for="city in savedCities" :key="city.id">
    <CityCard :city="city" @click="goToCityView(city)" />
  </div>
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
    const request = []
    savedCities.value.forEach((city) => {
      request.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=0cc9e70ed1c45d1f75ad22b3365aba0c&units=imperial`
        )
      )
    })

    const waetherData = await Promise.all(request)
    waetherData.forEach((value, index) => {
      savedCities.value[index].waether = value.data
    })
  }
}
await getCities()
const router = useRouter()
const goToCityView = (city) => {
  router.push({
    name: 'cityView',
    params: {
      state: city.state,
      city: city.city
    },
    query: { lat: city.coords.lat, lon: city.coords.lon }
  })
}
</script>
