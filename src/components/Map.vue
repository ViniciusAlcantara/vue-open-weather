<template>
  <div ref="mapDiv" id="map" style="width: 100%; height: 50vh;" :class="`map q-mx-xs ${$q.screen.lt.md ? '' : 'fit'}`" />
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue'

export default defineComponent({
  props: ['lat', 'lng'],
  setup (props) {
    const center = ref({ lat: props.lat, lng: props.lng })
    const mapDiv = ref(null)
    const map = ref(null)
    const marker = ref(null)

    onMounted(async () => {
      map.value = new window.google.maps.Map(mapDiv.value, {
        center: center.value,
        zoom: 14
      })

      marker.value = new window.google.maps.Marker({
        position: center.value,
        map: map.value
      })
    })

    return {
      center,
      mapDiv
    }
  }
})
</script>
