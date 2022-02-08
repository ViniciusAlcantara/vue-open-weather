<template>
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

      <q-item v-if="weather">
        <q-item-section>
          <q-item-label>
            <q-skeleton type="text" />
          </q-item-label>
          <q-item-label caption>
            <q-skeleton type="text" />
          </q-item-label>
        </q-item-section>
      </q-item>

      <q-skeleton height="200px" square v-else />

      <q-card-actions v-if="weather" align="right" class="q-gutter-md">
        <q-skeleton type="QBtn" />
      </q-card-actions>
    </q-card>
</template>

<script>
export default {
  props: ['weather'],
  setup () {
  }
}
</script>
