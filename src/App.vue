<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container class="q-pa-md">
      <div class="row">
        <div class="col-12 q-pa-sm">
          <location-autocomplete
            @place="setLocationInfo"
            @clear="clear"
            :location="location"
            label="Location"
            :rules="[ val => val && val.length > 0 || 'Please type the location']"
            hint="i.e.: Brussels, Belgium"
          />
        </div>
      </div>

      <div class="row" v-if="location && !error">
        <div class="col-12 col-md-6 col-lg-6 col-xl-6 q-pa-sm">
          <weather-card
            :location="location"
            :current="currentWeather"
            :loading="loading"
          />

          <div class="row">
            <div class="col-12 col-md-6 col-lg-6 col-xl-6 q-pa-xs">
              <history-weather
                title="Next 7 days"
                :history="nextDaysWeather"
                :loading="loading"
              />
            </div>

            <div class="col-12 col-md-6 col-lg-6 col-xl-6 q-pa-xs">
              <history-weather
                title="Last 5 days"
                :history="historyWeatherInfo"
                :past="true"
                :loading="loading"
              />
            </div>
          </div>
        </div>

        <div class="col-12 col-md-6 col-lg-6 col-xl-6 q-pa-sm">
          <Map
            :lat="lat"
            :lng="lng"
            :key="location"
          />
        </div>
      </div>

      <div class="row" v-else-if="error">
        <div class="q-pa-sm full-width">
          <q-banner inline-actions class="dense text-white bg-red full-width">
            <template v-slot:avatar>
              <q-icon name="error" color="white" />
            </template>
            {{error}}
            <template v-slot:action>
              <q-btn flat color="white" label="Dismiss"  @click="error = null" />
            </template>
          </q-banner>
        </div>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script>
import { inject, ref } from 'vue'
import LocationAutocomplete from './components/LocationAutocomplete'
import Map from './components/Map.vue'
import WeatherCard from './components/WeatherCard.vue'
import HistoryWeather from './components/HistoryWeather.vue'

export default {
  name: 'Home',
  components: {
    LocationAutocomplete,
    Map,
    WeatherCard,
    HistoryWeather
  },
  setup () {
    const location = ref('')
    const lat = ref(null)
    const lng = ref(null)
    const currentWeather = ref(null)
    const todayWeather = ref(null)
    const nextDaysWeather = ref([])
    const historyWeatherInfo = ref([])
    const loading = ref(false)
    const error = ref(null)

    const axios = inject('axios') // inject axios

    const setLocationInfo = (locationInfo) => {
      location.value = locationInfo.location
      lat.value = locationInfo.locationLat
      lng.value = locationInfo.locationLng
      getWeatherInfo()
    }

    const getWeatherInfo = async () => {
      try {
        loading.value = true
        const weatherInfo = await axios.get(
          `${process.env.VUE_APP_OPENWEATHER_URL}/onecall?lat=${lat.value}&lon=${lng.value}&exclude=minutely,hourly&units=metric&appid=${process.env.VUE_APP_OPENWEATHER_KEY}`
        )

        const { daily, current } = weatherInfo.data
        currentWeather.value = { ...current, ...daily[0], currentTemp: current.temp } // join all info needed in one object
        nextDaysWeather.value = daily.slice(1, daily.length) // remove day 0 because day 0 is the current day

        const promises = []
        for (let i = 1; i <= 5; i++) {
          let day = new Date()
          day.setDate(day.getDate() - i)
          day = Math.floor(day.getTime() / 1000) // convert to the time expected by the open weather API (unix timestamp)
          promises.push(axios.get(
            `${process.env.VUE_APP_OPENWEATHER_URL}/onecall/timemachine?lat=${lat.value}&lon=${lng.value}&dt=${day}&units=metric&appid=${process.env.VUE_APP_OPENWEATHER_KEY}`
          ))
        }

        const promisesResult = await Promise.all(promises)
        historyWeatherInfo.value = promisesResult.map(p => p.data?.current) // map the promises results and get only the needed data
        error.value = null
      } catch (err) {
        error.value = err.message
      } finally {
        loading.value = false
      }
    }
    const clear = () => {
      error.value = null
      location.value = null
      lat.value = null
      lng.value = null
    }

    return {
      location,
      lat,
      lng,
      clear,
      currentWeather,
      error,
      historyWeatherInfo,
      loading,
      todayWeather,
      nextDaysWeather,
      setLocationInfo
    }
  }
}
</script>

<style>
html, body {
  width: 100%;
  height: 100%;
  margin: 0px;
  padding: 0px;
}
</style>
