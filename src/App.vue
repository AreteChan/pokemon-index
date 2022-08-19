<template>
  <div id="app">
    <img src="./assets/pokeapi.png">
    <Number/>
    <List/>
  </div>
</template>

<script>
import Number from './components/Number.vue'
import List from './components/List.vue'

export default {
  name: 'App',
  components: { Number, List },
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
  }
  img {
    display: block;
    margin: 20px auto;
  }
</style>
