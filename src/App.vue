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
    },
    provide () {
      return {
        // provide API consts to children
        API: {
          _BASEURL: process.env.VUE_APP_API_BASEURL,
          _VERSION: `api-version=${process.env.VUE_APP_API_VERSION}`
        }
      }
    }
  }
</script>

<style lang="scss">
  @import './scss/base.scss'; // Import app styling
</style>
