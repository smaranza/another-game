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
          _baseurl: process.env.VUE_APP_API_BASEURL,
          _version: `api-version=${process.env.VUE_APP_API_VERSION}`,
          _schema: {
            'colors' : ['none', 'yellow', 'red', 'green', 'blue']
          }
        }
      }
    }
  }
</script>

<style lang="scss">
  @import './scss/base.scss'; // Import app styling
</style>
