<!--
Apply scaling or transforming to elements.

Usage:

<Pack :scale="0.5">
  <YourElements />
</Pack>
-->

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  scale?: number | string
  origin?: string
}>()

const style = computed(() => {
  const transforms = []
  let width;
  if (props.scale != null) {
    transforms.push(`scale(${props.scale || 1})`)
    width = `${100.0/(props.scale || 1)}%`
  } else {
    width = "100%"
  }

  return {
    'width': width,
    'transform': transforms.join(' '),
    'transform-origin': props.origin || 'top left',
  }
})
</script>

<template>
  <div :style="style">
    <slot />
  </div>
</template>