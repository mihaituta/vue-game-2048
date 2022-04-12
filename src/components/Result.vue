<template>
  <h2 class="result" v-if="result !== ''">{{ result }}</h2>
</template>

<script setup>
defineProps(['result'])
</script>

<style scoped>
.result {
  margin-bottom: 1rem;
  font-size: 2rem;
}
</style>
