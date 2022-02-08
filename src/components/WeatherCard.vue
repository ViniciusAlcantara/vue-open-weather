<template>
  <div v-if="loading">
    <q-card class="q-mx-xs">
      <q-item>
        <q-item-section>
          <q-item-label>
            <q-skeleton type="text" />
          </q-item-label>
          <q-item-label caption>
            <q-skeleton type="text" />
          </q-item-label>
        </q-item-section>
      </q-item>

      <q-item>
        <q-item-section>
          <q-item-label>
            <q-skeleton type="text" />
          </q-item-label>
          <q-item-label caption>
            <q-skeleton type="text" />
          </q-item-label>
        </q-item-section>
      </q-item>

      <q-card-actions align="right" class="q-gutter-md">
        <q-skeleton type="QBtn" />
      </q-card-actions>
    </q-card>
  </div>

  <q-card v-else class="q-mx-xs">
    <q-card-section>
      <q-item v-ripple>
        <q-item-section>
          <q-item-label class="text-subtitle1 text-weight-thin">{{location}}</q-item-label>
          <q-item-label class="text-h6" lines="1"> {{current?.weather[0]?.main}}: {{current?.weather[0]?.description}}</q-item-label>
        </q-item-section>
      </q-item>

      <div class="row flex justify-between">
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
          <div class="row flex justify-between">
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
export default {
  components: { Item },
  props: ['location', 'current', 'loading'],
  setup () {
    const expanded = ref(false)
    const iconsUrl = ref(process.env.VUE_APP_OPENWEATHER_ICONS_URL)

    const formatDate = (date) => {
      const formatedDate = new Date(date * 1000)
      return formatedDate.getHours() + ':' + formatedDate.getMinutes()
    }

    return {
      expanded,
      iconsUrl,
      formatDate
    }
  }
}
</script>
