<template>
  <div class="container">
    <h1 class="title">Vue 2048</h1>
    <board />
    <p class="how-to-play">
      <span>HOW TO PLAY:</span> Use your <span>arrow keys </span> to move the
      tiles. Tiles with the same number <span>merge into one</span> when they
      touch. Add them up to reach <span>2048!</span>
    </p>
  </div>
</template>

<script>
export default {
  props: {},
  components: {
    board: require('./Board').default,
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 400px;

  .title {
    margin: 2rem 0 1rem 0;
    font-size: 3.5rem;
  }

  .how-to-play {
    font-size: 1.1rem;
    margin: 2rem 0 2rem 0;
    line-height: 1.65;
  }
}

span {
  font-weight: bold;
  font-size: 1.2rem;
}
</style>
