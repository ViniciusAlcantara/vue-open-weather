<template>
    <q-card class="q-my-sm">
      <q-card-section>
        <div class="text-h6">{{title}}</div>
        <div v-if="past">
          <item
            v-for="(item, index) in history"
            :key="index" :label="`${getDay(item.dt)} - ${item.weather[0]?.main} - ${item.temp}&deg;C`"
            :icon="`${iconsUrl}${item.weather[0].icon}@4x.png`"
            :image="true"
          />
        </div>
        <div v-else>
          <item
            v-for="(item, index) in history"
            :key="index" :label="`${getDay(item.dt)} - ${item.weather[0]?.main} - ${item.temp.max}&deg;C / ${item.temp.min}&deg;C`"
            :icon="`${iconsUrl}${item.weather[0].icon}@4x.png`"
            :image="true"
          />
        </div>
      </q-card-section>
    </q-card>
</template>

<script>
import { ref } from '@vue/reactivity'
import Item from './Item.vue'
export default {
  components: { Item },
  props: ['history', 'title', 'past'],
  setup () {
    const weekday = ref(['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'])
    const iconsUrl = ref(process.env.VUE_APP_OPENWEATHER_ICONS_URL)

    const getDay = (date) => {
      const formatedDate = new Date(date * 1000)
      return weekday.value[formatedDate.getDay()]
    }

    return {
      getDay,
      iconsUrl
    }
  }
}
</script>
