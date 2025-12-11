<script lang="ts" setup>
import {onMounted, ref} from 'vue';
import Download from './Download/index.vue'
import Read from './Read/index.vue'
import Shell from './Shell/index.vue'

const route = ref('')
const enterAction = ref({})

onMounted(() => {
  window.utools.onPluginEnter((action) => {
    route.value = action.code
    enterAction.value = action
  })
  window.utools.onPluginOut((isKill) => {
    route.value = ''
  })
})
</script>

<template>
  <template v-if="route === 'download'">
    <Download :enterAction="enterAction"></Download>
  </template>
  <template v-if="route === 'shell'">
    <Shell :enterAction="enterAction"></Shell>
  </template>

  <template v-if="route === 'read'">
    <Read :enterAction="enterAction"></Read>
  </template>
</template>
