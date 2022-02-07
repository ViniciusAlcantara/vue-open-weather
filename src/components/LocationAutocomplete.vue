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
import { watch } from '@vue/runtime-core'
// Add google map script if not exist; if already exist, return true
window.checkAndAttachMapScript = function (callback) {
  const scriptId = 'map-api-script'
  const mapAlreadyAttached = !!document.getElementById(scriptId)

  if (mapAlreadyAttached) {
    if (window.google) { // Script attached but may not finished loading; so check for 'google' object.
      callback()
    }
  } else {
    window.mapApiInitialized = callback

    const script = document.createElement('script')
    script.id = scriptId
    script.src = `https://maps.googleapis.com/maps/api/js?key=${process.env.VUE_APP_MAPS_KEY}&libraries=places,geometry&callback=mapApiInitialized`
    document.body.appendChild(script)
  }

  return mapAlreadyAttached
}

export default {
  props: ['hint', 'rules', 'address', 'label'],
  setup (props, context) {
    const autocompleteA = ref(null)
    const autocomplete = ref(null)
    const location = ref(props.address)

    watch(autocomplete, (autocomplete, oldAutocomplete) => {
      if (autocomplete && !oldAutocomplete) window.checkAndAttachMapScript(autocompleteInit)
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
