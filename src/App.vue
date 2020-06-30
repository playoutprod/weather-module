<template>
  <div id="app">
    <WeatherModule/>
  </div>
</template>

<script>
import WeatherModule from './components/WeatherModule.vue'

export default {
  name: 'App',
  components: {
    WeatherModule
  }
}
</script>

<style>

#app {
  width:100%;
  height:100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
