<template>
  <Main />
</template>

<script>
import Main from './components/MainView.vue'

export default {
  name: 'App',
  components: {
    Main,
  },
}
</script>

<style lang="scss">
html {
  height: 100%;
  background-color: $primary;
}

* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  justify-content: center;
  color: $text-primary;
}
</style>
