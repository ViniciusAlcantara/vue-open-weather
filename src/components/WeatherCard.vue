<template>
  <div v-if="loading">
    <skeleton weather />
  </div>

  <q-card v-else class="q-mx-xs">
    <q-card-section>
      <q-item v-ripple>
        <q-item-section>
          <q-item-label class="text-subtitle1 text-weight-thin">{{location}}</q-item-label>
          <q-item-label class="text-h6" lines="1"> {{current?.weather[0]?.main}}: {{titleCase(current?.weather[0]?.description)}}</q-item-label>
        </q-item-section>
      </q-item>

      <div :class="`${$q.screen.lt.md ? '' : 'row flex justify-between'}`">
        <item :label="`${current?.currentTemp}&deg;C`" :icon="`${iconsUrl}${current?.weather[0]?.icon}@4x.png`" :image="true" />

        <item :label="`${current?.temp?.max}&deg;C`" icon="arrow_upward" caption="Max" color="red" />

        <item :label="`${current?.temp?.min}&deg;C`" icon="arrow_downward" caption="Min" color="blue" />
      </div>
    </q-card-section>

    <q-card-actions>
      <q-space />
      <q-btn
        color="grey"
        flat
        dense
        :icon="expanded ? 'keyboard_arrow_up' : 'keyboard_arrow_down'"
        @click="expanded = !expanded"
      >
        See details
      </q-btn>
    </q-card-actions>

    <q-slide-transition>
      <div v-show="expanded">
        <q-separator />

        <q-card-section>
          <div :class="`${$q.screen.lt.md ? '' : 'row flex justify-between'}`">
            <item :label="`${current?.wind_speed} m/s`" icon="send" caption="Wind Speed" />

            <item :label="`${current?.humidity} %`" icon="water_drop" caption="Humidity" />

            <item :label="`${current?.pressure}hPa`" icon="vertical_align_center" caption="Pressure" />

            <item :label="formatDate(current?.sunrise)" icon="brightness_low" caption="Sunrise Time" />

            <item :label="formatDate(current?.sunset)" icon="brightness_4" caption="Sunset Time" />
          </div>
        </q-card-section>
      </div>
    </q-slide-transition>
  </q-card>
</template>

<script>
import { ref } from '@vue/reactivity'
import Item from './Item.vue'
import Skeleton from './Skeleton'
export default {
  components: { Item, Skeleton },
  props: ['location', 'current', 'loading'],
  setup () {
    const expanded = ref(false)
    const iconsUrl = ref(process.env.VUE_APP_OPENWEATHER_ICONS_URL)

    const formatDate = (date) => {
      const formatedDate = new Date(date * 1000) // convert unix timestamp to js timestamp and generate a new Data
      return formatedDate.getHours() + ':' + formatedDate.getMinutes()
    }

    const titleCase = (str) => {
      if (!str) return ''

      const strs = str.split(' ')

      const titleCase = strs.map(s => {
        const title = s.split('')
        title[0] = title[0].toUpperCase()
        return title.join('')
      })

      return titleCase.join(' ')
    }

    return {
      expanded,
      iconsUrl,
      formatDate,
      titleCase
    }
  }
}
</script>
