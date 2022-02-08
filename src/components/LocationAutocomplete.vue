<template>
    <q-input
      outlined
      color="primary"
      type="text"
      required
      :loading="loading"
      clearable
      v-model="location"
      class="autocomplete-adress"
      minlength="3"
      :label="label"
      @clear="clear"
      ref="autocomplete"
      :hint="hint"
      lazy-rules
    />
</template>

<script>
import { ref } from '@vue/reactivity'
import { Loader } from '@googlemaps/js-api-loader'
import { onMounted } from '@vue/runtime-core'

export default {
  props: ['hint', 'rules', 'address', 'label'],
  setup (props, context) {
    const autocompleteA = ref(null)
    const autocomplete = ref(null)
    const location = ref(props.address)
    const loading = ref(false)
    const loader = new Loader({ // loads google maps script
      apiKey: process.env.VUE_APP_MAPS_KEY,
      libraries: ['places', 'geometry', 'geocoder']
    })

    onMounted(async () => {
      await loader.load()
      autocompleteInit()
    })

    const autocompleteInit = () => {
      autocompleteA.value = new window.google.maps.places.Autocomplete( // initiate google maps autocomplete
        (autocomplete.value.getNativeElement()),
        { types: ['geocode'] }
      )

      autocompleteA.value.addListener('place_changed', setLocation)
      currentLocation()
    }

    const currentLocation = () => {
      loading.value = true
      window.navigator.geolocation.getCurrentPosition(res => {
        const geocoder = new window.google.maps.Geocoder()
        const lat = res.coords.latitude
        const lng = res.coords.longitude
        geocoder.geocode({ latLng: { lat, lng } },
          (results, status) => {
            loading.value = false
            // This is checking to see if the Geoeode Status is OK before proceeding
            if (status === window.google.maps.GeocoderStatus.OK) {
              location.value = results[0].formatted_address
              updateParentLocation(lat, lng)
            }
          })
      })
    }

    const setLocation = () => { // executed when place change
      const place = autocompleteA.value.getPlace()

      location.value = place.formatted_address

      updateParentLocation(place.geometry.location.lat(), place.geometry.location.lng())
    }

    const updateParentLocation = (lat, lng) => {
      const evtData = {
        location: location.value,
        locationLat: lat,
        locationLng: lng
      }

      context.emit('place', evtData) // send data to parent
    }

    const clear = () => {
      context.emit('clear')
    }

    return { location, autocomplete, autocompleteA, clear, loading }
  }
}
</script>
