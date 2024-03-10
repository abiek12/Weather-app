<template>
  <div></div>
</template>

<script setup>
import { Axios } from 'axios'
import { useRouter } from 'vue-router'

const weatherAPIKey = '0cc9e70ed1c45d1f75ad22b3365aba0c'
const route = useRouter()
const getWeatheraData = async () => {
  const lat = route.query.lat
  const lon = route.query.lon
  try {
    const weatherData = await Axios.get(
      `https://api.openweathermap.org/data/3.0/onecall?lat=${lat}&lon=${lon}&exclude={part}&appid=${weatherAPIKey}&units=imperial`
    )
    return weatherData
  } catch (error) {
    console.log(error)
  }
}

const weatherData = await getWeatheraData()
console.log(weatherData)
</script>
