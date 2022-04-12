<template>
  <div v-for="tile in boardTiles" :class="`tile x${tile}`">
    <div v-show="tile !== 0">{{ tile }}</div>
  </div>
</template>

<script setup>
defineProps(['boardTiles'])
</script>

<style scoped lang="scss">
.tile {
  width: 90px;
  height: 90px;
  background-color: hsl(0, 0%, 49%);
  border: 5px solid hsl(0, 0%, 30%);
  border-radius: 0.3rem;

  font-size: 2rem;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
}

.x2 {
  background-color: hsl(152, 47%, 80%);
  color: hsl(0, 0%, 52%);
}

.x4 {
  background-color: hsl(162, 32%, 52%);
  color: hsl(0, 0%, 100%);
}

.x8 {
  background-color: hsl(156, 53%, 45%);
  color: hsl(0, 0%, 100%);
}

.x16 {
  background-color: hsl(154, 100%, 41%);
  color: hsl(0, 0%, 100%);
}

.x32 {
  background-color: hsl(131, 52%, 40%);
  color: hsl(0, 0%, 100%);
}

.x64 {
  background-color: hsl(130, 90%, 41%);
  color: hsl(0, 0%, 98%);
}

.x128 {
  background-color: hsl(179, 47%, 80%);
  color: hsl(0, 0%, 52%);
}

.x256 {
  background-color: hsl(179, 40%, 61%);
  color: hsl(0, 0%, 100%);
}

.x512 {
  background-color: hsl(181, 49%, 43%);
  color: hsl(0, 0%, 100%);
}

.x1024 {
  background-color: hsl(181, 100%, 58%);
  color: hsl(0, 0%, 52%);
}

.x2048 {
  background-color: hsl(358, 85%, 63%);
  color: hsl(0, 0%, 100%);
}
</style>
