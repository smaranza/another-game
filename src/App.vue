<template>
  <AnotherHeader :list="fullList" />
  <div class="container px-0">
    <AnotherCatalogue @listUpdate="populateSearch" />
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

  data() {
    return {
      fullList: [],
      // provide API consts to children
      API: {
        _BASEURL: process.env.VUE_APP_API_BASEURL,
        _VERSION: `api-version=${process.env.VUE_APP_API_VERSION}`,
        _BEARER: null,
        _schema: {
          'colors': ['None', 'Yellow', 'Red', 'Green', 'Blue', 'Black']
        }
      }
    }
  },

  provide() {
    return {
      API: this.API,
      genres: {
        'None': 'None',
        'Yellow': 'Platform',
        'Red': 'Simulator',
        'Green': 'Open World',
        'Blue': 'Adventure',
        'Black': 'Horror'
      }
    }
  },

  methods: {
    async authorizeRequests(username, password) {
      const requestOptions = {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          "clientId": username,
          "clientSecret": password
        }
      )};

      // WIP
      this.API._BEARER = await (
        await (await fetch(`${process.env.VUE_APP_API_BASEURL}/credentials/authorize?api-version=2`, requestOptions)).json()
      );
      //     ).then(function(data) {
      //     return data;
      // });
    },

    populateSearch(list) {
      this.fullList = list;
    }
  },

  mounted() {
    // if (this.API._VERSION == 2) {
      this.authorizeRequests(process.env.VUE_APP_API_CLIENT_ID, process.env.VUE_APP_API_CLIENT_SECRET);
    // }
  },
}
</script>

<style lang="scss">@import './scss/base.scss'; // Import app styling</style>
