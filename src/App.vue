<template>
  <div id="app">
    <Task msg="Its your task manager" />
  </div>
</template>

<script>
/* eslint-disable */
import Task from './components/Task.vue'

export default {
  name: 'App',
  components: {
    Task
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>