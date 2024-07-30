<script setup>
const props = defineProps({
  url: {
    type: String,
    required: true
  }
})
</script>

<template>
  <a :href="url">Hint <i class="light-icon-external-link"></i></a>
</template>

<style>
</style>
