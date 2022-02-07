<template>
    <q-card>
      <q-card-section>
        <q-item class="q-my-sm" v-ripple>
          <q-item-section>
            <q-item-label >San Francisco</q-item-label>
            <q-item-label caption lines="1"> Mostly sunny</q-item-label>
          </q-item-section>
        </q-item>

        <div class="row q-px-md">
          <item label="23&deg;C" icon="http://openweathermap.org/img/wn/10d@2x.png" :image="true" />

          <item label="23&deg;C" icon="cloud_upload" caption="Max for Today" />

          <item label="19&deg;C" icon="cloud_download" caption="Min for Today" />
        </div>
      </q-card-section>

      <q-card-actions>
        <q-space />
        <q-btn
          color="grey"
          round
          flat
          dense
          :icon="expanded ? 'keyboard_arrow_up' : 'keyboard_arrow_down'"
          @click="expanded = !expanded"
        />
      </q-card-actions>

      <q-slide-transition>
        <div v-show="expanded">
          <q-separator />

          <q-card-section class="q-px-md">
            <div class="row">
              <item label="23 km/h" icon="send" caption="Wind Speed" />

              <item label="48%" icon="water_drop" caption="Humidity" />

              <item label="1016hPa" icon="vertical_align_center" caption="Pressure" />
            </div>

            <div class="row">
              <item label="06:00AM" icon="brightness_low" caption="Sunrise Time" />

              <item label="06:00PM" icon="brightness_4" caption="Sunset Time" />
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
  props: ['location'],
  setup () {
    const expanded = ref(false)

    return { expanded }
  }
}
</script>
