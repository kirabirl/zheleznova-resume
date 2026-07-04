<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  src: { type: String, required: true },
  alt: { type: String, required: true },
  ratio: { type: String, default: 'wide' },
})

const failed = ref(false)
watch(() => props.src, () => { failed.value = false })
</script>

<template>
  <div class="asset-frame" :class="`asset-frame--${ratio}`">
    <img v-if="!failed" :src="src" :alt="alt" @error="failed = true" />
    <div v-else class="asset-frame__fallback" aria-hidden="true">
      <span>{{ alt }}</span>
    </div>
  </div>
</template>
