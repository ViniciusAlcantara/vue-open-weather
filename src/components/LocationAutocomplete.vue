<template>
    <q-input
      outlined
      color="primary"
      type="text"
      required
      v-model="location"
      class="autocomplete-adress"
      minlength="3"
      :label="label"
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
    const loader = new Loader({
      apiKey: process.env.VUE_APP_MAPS_KEY,
      libraries: ['places', 'geometry']
    })

    onMounted(async () => {
      await loader.load()
      autocompleteInit()
    })

    const autocompleteInit = () => {
      autocompleteA.value = new window.google.maps.places.Autocomplete(
        (autocomplete.value.getNativeElement()),
        { types: ['geocode'] }
      )

      autocompleteA.value.addListener('place_changed', setLocation)
    }

    const setLocation = () => {
      const place = autocompleteA.value.getPlace()

      location.value = place.formatted_address

      const evtData = {
        location: location.value,
        locationLat: place.geometry.location.lat(),
        locationLng: place.geometry.location.lng()
      }

      context.emit('place', evtData)
    }

    return { location, autocomplete, autocompleteA }
  }
}
</script>
