<template>
  <div v-if="isEnabled" class="ad-container">

    <ins ref="adRef" class="adsbygoogle"
         style="display:block"
         data-ad-client="ca-pub-1238777311285568"
         data-ad-slot="8138079461"
         data-ad-format="auto"
         data-full-width-responsive="true"></ins>
  </div>
</template>

<script setup lang="ts">
import { nextTick, onMounted, onUnmounted, ref } from 'vue'
import { cancelAdPush, isAdHostAllowed, scheduleAdPush } from '../composables/useAdsense'

const isEnabled = ref(false)
const adRef = ref<HTMLElement | null>(null)

onMounted(async () => {
  if (!isAdHostAllowed()) {
    return
  }

  isEnabled.value = true

  await nextTick()

  if (adRef.value) {
    scheduleAdPush(adRef.value)
  }
})

onUnmounted(() => {
  if (adRef.value) {
    cancelAdPush(adRef.value)
  }
})
</script>

<style scoped>
.ad-container {
  margin: 2rem 0;
  padding: 1rem 0;
  border-top: 1px solid var(--vp-c-divider);
}


</style>
