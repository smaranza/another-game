<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld user="User"/>
  <h3>Subtitle <i class="bi-play-circle"></i></h3>
</template>

<script>
  import HelloWorld from './components/HelloWorld.vue'

  export default {
    name: 'App',
    components: {
      HelloWorld
    }
  }
</script>

<style lang="scss">
  @import './scss/app.scss'; // Import app styling
</style>
