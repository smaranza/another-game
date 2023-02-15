<template>
  <AnotherHeader user="User"/>
  <div class="container px-0">
      <AnotherCatalogue />
  </div>
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
          _gittest: process.env.VUE_APP_GIT_TEST,
          _schema: {
            'colors' : ['None', 'Yellow', 'Red', 'Green', 'Blue', 'Black']
          }
        }
      }
    }
  }
</script>

<style lang="scss">
  @import './scss/base.scss'; // Import app styling
</style>
