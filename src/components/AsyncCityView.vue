<template>
  <div class="flex flex-col flex-1 items-center">
    <!-- Banner -->
    <div
      v-if="route.currentRoute.value.query.preview"
      class="text-white p-4 bg-weather-secondary w-full text-center"
    >
      <p>You are currently previewing this city, click the "+" icon to start tracking this city.</p>
    </div>
    <!-- Weather Overview -->
    <div class="flex flex-col items-center text-white py-12">
      <h1 class="text-4xl mb-2">{{ route.currentRoute.value.params.city }}</h1>
      <p class="text-sm mb-8">
        {{
          new Date(weatherData.data.currentTime).toLocaleDateString('en-us', {
            weekday: 'short',
            day: '2-digit',
            month: 'long'
          })
        }},
        {{
          new Date(weatherData.data.currentTime).toLocaleTimeString('en-us', {
            timeStyle: 'short'
          })
        }}
      </p>
      <p class="text-8xl mb-8">{{ Math.round(weatherData.data.list[0].main.temp) }}&deg;</p>
      <p>
        Feels like
        {{ Math.round(weatherData.data.list[0].main.feels_like) }} &deg;
      </p>
      <p class="capitalize">
        {{ weatherData.data.list[0].weather[0].description }}
      </p>
      <img
        class="w-[150px] h-auto"
        :src="`http://openweathermap.org/img/wn/${weatherData.data.list[0].weather[0].icon}@2x.png`"
        alt=""
      />
    </div>
  </div>
</template>

<script setup>
import axios from 'axios'
import { useRouter } from 'vue-router'

const weatherAPIKey = '0cc9e70ed1c45d1f75ad22b3365aba0c'
const route = useRouter()
const getWeatheraData = async () => {
  const lat = route.currentRoute.value.query.lat
  const lon = route.currentRoute.value.query.lon
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${weatherAPIKey}`
    )
    // cal current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000
    const utc = weatherData.data.list[0].dt * 1000 + localOffset
    weatherData.data.currentTime = utc + 1000 * weatherData.data.city.timezone
    // cal hourly weather offset
    weatherData.data.list.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset
      hour.currentTime = utc + 1000 * weatherData.data.city.timezone
    })
    return weatherData
  } catch (error) {
    console.log(error)
  }
}

const weatherData = await getWeatheraData()
console.log(weatherData)
</script>
