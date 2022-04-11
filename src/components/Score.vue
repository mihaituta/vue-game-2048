<template>
  <div class="top-display">
    <button class="btn-new-game" @click="restartGame">New Game</button>
    <div class="score-container">
      <div class="score__label">Score</div>
      <div class="score__numbers">{{ score }}</div>
    </div>
  </div>
</template>

<script setup>
defineProps(['score'])
const emit = defineEmits(['newGame'])
const restartGame = () => emit('newGame')
</script>

<style scoped lang="scss">
.top-display {
  width: 100%;
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.3rem;

  .btn-new-game {
    padding: 0.8rem;
    margin-bottom: 1.5rem;
    background-color: $text-primary;
    border: none;
    outline: none;
    font-size: 1.2rem;
    font-weight: bold;
    border-radius: 0.3rem;
    color: hsl(0, 0%, 100%);
    cursor: pointer;
    transition: 200ms;
    &:hover {
      background-color: hsl(159, 47%, 61%);
    }
  }

  .score-container {
    padding: 0.5rem;
    margin-bottom: 1.5rem;
    background-color: $text-primary;
    border: none;
    outline: none;
    font-size: 1.2rem;
    font-weight: bold;
    border-radius: 0.3rem;
    color: hsl(0, 0%, 100%);
    text-align: center;
    display: flex;
    flex-direction: column;

    .score__label {
      margin-bottom: 0.2rem;
    }
    .score__numbers {
      font-size: 1.5rem;
    }
  }
}
</style>
