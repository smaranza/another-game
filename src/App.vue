<template>
  <AnotherHeader user="User"/>
  <AnotherCatalogue />
</template>

<script>
  import AnotherHeader from './components/AnotherHeader.vue'
  import AnotherCatalogue from './components/AnotherCatalogue.vue'

  export default {
    name: 'App',
    components: {
      AnotherHeader,
      AnotherCatalogue,
    }
  }
</script>

<style lang="scss">
  @import './scss/base.scss'; // Import app styling
</style>
