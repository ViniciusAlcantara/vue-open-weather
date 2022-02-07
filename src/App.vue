<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container class="q-pa-md">
      <div class="row q-pa-md">
        <div class="col-12 q-pa-sm">
          <location-autocomplete @place="setLocationInfo" :location="location" :key="locationKey" label="Location"
            :rules="[ val => val && val.length > 0 || 'Please type the location']"
            hint="i.e.: Brussels, Belgium"
          />
        </div>
      </div>
      <div class="row q-px-md">
        <div class="col-12 col-md-6 col-lg-6 col-xl-6 q-pa-sm">
          <weather-card :location="location" />
        </div>
        <div class="col-12 col-md-6 col-lg-6 col-xl-6 q-pa-sm">
          <!-- <Map :lat="lat" :lng="lng" /> -->
        </div>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script>
import { inject, ref } from 'vue'
import LocationAutocomplete from './components/LocationAutocomplete'
// import Map from './components/Map.vue'
import WeatherCard from './components/WeatherCard.vue'

export default {
  name: 'Home',
  components: {
    LocationAutocomplete,
    // Map,
    WeatherCard
  },
  setup () {
    const locationKey = ref(0)
    const location = ref('')
    const lat = ref(40.689247)
    const lng = ref(-74.044502)
    const historyWeatherInfo = ref([])

    const axios = inject('axios') // inject axios

    const setLocationInfo = (locationInfo) => {
      location.value = locationInfo.location
      lat.value = locationInfo.locationLat
      lng.value = locationInfo.locationLng
      console.log('Location Info:', locationInfo)
      getWeatherInfo()
    }

    const getWeatherInfo = async () => {
      try {
        const weatherInfo = await axios.get(`${process.env.VUE_APP_OPENWEATHER_URL}/onecall?lat=${lat.value}&lon=${lng.value}&exclude=minutely,hourly&appid=${process.env.VUE_APP_OPENWEATHER_KEY}`)
        console.log('Weather Info:', weatherInfo.data)

        const promises = []
        for (let i = 1; i <= 5; i++) {
          let day = new Date()
          day.setDate(day.getDate() - i)
          day = Math.floor(day.getTime() / 1000) // convert to the time expected by the open weather API
          promises.push(axios.get(`${process.env.VUE_APP_OPENWEATHER_URL}/onecall/timemachine?lat=${lat.value}&lon=${lng.value}&dt=${day}&appid=${process.env.VUE_APP_OPENWEATHER_KEY}`))
        }

        historyWeatherInfo.value = Promise.all(promises)
        console.log(historyWeatherInfo)
      } catch (error) {
        // TODO add error treatment
        console.log(error)
      }
    }

    return {
      locationKey,
      location,
      lat,
      lng,
      setLocationInfo
    }
  }
}
</script>
