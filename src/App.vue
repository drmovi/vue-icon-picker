<template>
  <div id="app">

    <IconPicker/>
  </div>
</template>

<script>
import IconPicker from './components/IconPicker.vue'

export default {
  name: 'app',
  components: {
      IconPicker
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
