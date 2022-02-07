<template>
    <q-card>
      <q-card-section>
        <div class="text-h6">{{title}}</div>
        <div class="row">
            <item
                v-for="(item, index) in history"
                :key="index" label="23&deg;C"
                icon="http://openweathermap.org/img/wn/10d@2x.png"
                :image="true"
            />
        </div>
      </q-card-section>
    </q-card>
</template>

<script>
import Item from './Item.vue'
export default {
  components: { Item },
    props: ['history', 'title'],
    setup() {
    },
}
</script>
