<template>
  <div class="app">
    <Calculator/>
  </div>
</template>

<script>
import Calculator from './components/Calculator';

export default {
  name: 'App',
  components: {
    Calculator,
  },
};
</script>

<style>
html,
body {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  background-color: #35495e;
}
.app {
  width: 100%;
  height: 100%;
  display: grid;
  justify-content: center;
  align-content: center;
}
</style>
