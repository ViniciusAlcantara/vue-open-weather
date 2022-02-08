<template>
  <div v-if="loading">
    <skeleton />
  </div>

  <q-card v-else class="full-height q-my-sm">
    <q-card-section>
      <div class="text-h6 q-px-md">{{title}}</div>
      <div v-if="past">
        <item
          v-for="(item, index) in history"
          :key="index" :label="`${getDay(item.dt)} - ${item.weather[0]?.main}`"
          :icon="`${iconsUrl}${item.weather[0].icon}@4x.png`"
          :caption="`${item.temp}&deg;C`"
          :image="true"
        />
      </div>

      <div v-else>
        <item
          v-for="(item, index) in history"
          :key="index" :label="`${getDay(item.dt)} - ${item.weather[0]?.main}`"
          :icon="`${iconsUrl}${item.weather[0].icon}@4x.png`"
          :caption="`${item.temp.max}&deg;C max / ${item.temp.min}&deg;C min`"
          :image="true"
        />
      </div>
    </q-card-section>
  </q-card>
</template>

<script>
import { ref } from '@vue/reactivity'
import Item from './Item'
import Skeleton from './Skeleton'
export default {
  components: { Item, Skeleton },
  props: ['history', 'title', 'past', 'loading'],
  setup () {
    const weekday = ref(['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'])
    const iconsUrl = ref(process.env.VUE_APP_OPENWEATHER_ICONS_URL)

    const getDay = (date) => {
      const formatedDate = new Date(date * 1000) // convert unix timestamp to js timestamp and generate a new Data
      return weekday.value[formatedDate.getDay()]
    }

    return {
      getDay,
      iconsUrl
    }
  }
}
</script>
