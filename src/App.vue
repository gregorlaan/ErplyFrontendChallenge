<template>
  <div id="app">
    <main-header></main-header>
    <router-view/>
  </div>
</template>

<script>
import MainHeader from '@/components/MainHeader'
import 'mdi-vue/EarthIcon'
import 'mdi-vue/CurrencyEurIcon'
import 'mdi-vue/CheckboxMarkedCircleOutlineIcon'

export default {
  name: 'App',
  components: {
    MainHeader
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
  }
  p.country .mdi-icon,
  p.availability .mdi-icon {
    vertical-align: text-bottom;
  }
</style>
