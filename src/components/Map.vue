<template>
  <GoogleMap
  :api-key="apiKey"
  style="width: 100%; height: 100%"
  :center="center"
  :zoom="15"
  >
    <Marker :options="{ position: center }" />
  </GoogleMap>
</template>

<script>
import { defineComponent } from 'vue'
import { GoogleMap, Marker } from 'vue3-google-map'

export default defineComponent({
  components: { GoogleMap, Marker },
  props: ['lat', 'lng'],
  setup (props) {
    const center = { lat: props.lat, lng: props.lng }
    const apiKey = process.env.VUE_APP_MAPS_KEY

    return { center, apiKey }
  }
})
</script>
