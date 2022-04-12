<template>
  <MainView />
</template>

<script>
import MainView from './components/MainView.vue'

export default {
  name: 'App',
  components: {
    MainView,
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
