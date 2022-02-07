<template>
    <q-card>
      <q-card-section>
        <q-item v-ripple>
          <q-item-section>
            <q-item-label class="text-h5">{{location}}</q-item-label>
            <q-item-label class="text-subtitle1 text-weight-thin" lines="1"> {{current?.weather[0]?.main}} </q-item-label>
          </q-item-section>
        </q-item>

        <div class="row">
          <item :label="`${current?.currentTemp}&deg;C`" :icon="`${iconsUrl}${current?.weather[0]?.icon}@4x.png`" :image="true" />

          <item :label="`${current?.temp?.max}&deg;C`" icon="cloud_upload" caption="Max" />

          <item :label="`${current?.temp?.min}19&deg;C`" icon="cloud_download" caption="Min" />
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
            <div class="row">
              <item :label="`${current?.wind_speed} m/s`" icon="send" caption="Wind Speed" />

              <item :label="`${current?.humidity} %`" icon="water_drop" caption="Humidity" />

              <item :label="`${current?.pressure}hPa`" icon="vertical_align_center" caption="Pressure" />
            </div>

            <div class="row">
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
  props: ['location', 'current'],
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
