<template>
  <div id="app">
    <router-view/>
    <app-footer></app-footer>
  </div>
</template>

<script>
import AppFooter from './components/AppFooter.vue'
export default {
  name: 'App',
  components: { AppFooter }
}
</script>

<style>
body {
  margin: 0;
  background: #0055aa;
  font-size: 16px;
  padding: 20px 100px 0px 100px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
