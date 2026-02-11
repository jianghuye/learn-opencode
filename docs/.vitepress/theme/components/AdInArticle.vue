<template>
  <div v-if="isEnabled" class="ad-in-article">
    <ins ref="adRef" class="adsbygoogle"
         style="display:block; text-align:center;"
         data-ad-layout="in-article"
         data-ad-format="fluid"
         data-ad-client="ca-pub-1238777311285568"
         data-ad-slot="9628105168"></ins>
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
.ad-in-article {
  margin: 2rem 0;
}
</style>
