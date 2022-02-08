<template>
  <div>
    <q-skeleton v-if="loading" height="height: 100%" square />

    <div ref="mapDiv" style="height: 100%; width: 100%;" id="map" class="map" />
  </div>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue'

export default defineComponent({
  props: ['lat', 'lng', 'loading'],
  setup (props) {
    const center = ref({ lat: props.lat, lng: props.lng })
    const mapDiv = ref(null)
    const map = ref(null)

    onMounted(async () => {
      map.value = new window.google.maps.Map(mapDiv.value, {
        center: center.value,
        zoom: 7
      })
    })

    return {
      center,
      mapDiv
    }
  }
})
</script>
