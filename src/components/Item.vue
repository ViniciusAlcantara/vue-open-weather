<template>
  <q-item class="q-px-xs" v-ripple dense>
    <q-item-section avatar class="q-pr-sm">
      <q-avatar rounded text-color="white" v-if="image">
        <q-img :src="icon" />
      </q-avatar>
      <q-avatar :icon="icon" :text-color="color ?? 'black'" v-else />
    </q-item-section>

    <q-item-section>
      <q-item-label> {{label}} </q-item-label>
      <q-item-label caption v-if="caption" lines="1"> {{caption}} </q-item-label>
    </q-item-section>
  </q-item>
</template>

<script>
export default {
  name: 'item',
  props: ['label', 'icon', 'image', 'caption', 'color'],
  setup () {
  }
}
</script>
