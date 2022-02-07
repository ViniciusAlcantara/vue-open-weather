<template>
    <q-item v-ripple dense>
        <q-item-section avatar>
            <q-avatar rounded text-color="white" v-if="image">
                <q-img
                    :src="icon"
                />
            </q-avatar>
            <q-avatar :icon="icon" v-else />
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
  props: ['label', 'icon', 'image', 'caption'],
  setup () {
  }
}
</script>
