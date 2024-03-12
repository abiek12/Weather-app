<template>
  <div class=""></div>
</template>

<script setup>
import axios from 'axios'
import { ref } from 'vue'

const savedCities = ref([])
const getCities = async () => {
  if (localStorage.getItem('savedCities')) {
    savedCities.value = JSON.parselocalStorage.getItem('savedCities')
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
</script>
