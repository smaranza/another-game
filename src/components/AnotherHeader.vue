<template>
  <nav class="navbar another--navbar theme-dark p-2 justify-content-center justify-content-md-between">
      <a class="navbar-brand d-flex text-uppercase align-flex-center" href="#">
        <i class="d-inline-block another--logo"></i>
        <span>nother-game</span>
      </a>
      <form class="d-flex" role="search" id="search">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success" @click="searchData">Search</button>
      </form>
  </nav>

  <div>{{ searchResultList }}</div>
</template>

<script>
  export default {
    name: 'AnotherHeader',
    props: {
      list: Object
    },

    inject: ['API'],

    data() {
      return {
        resultList: []
      }
    },

    computed: {
        searchResultList() {
          return this.resultList
      }
    },

    methods: {
      searchData(e) {
        e.preventDefault();
        e.stopPropagation();

        // searchTerm
        let searchTerm = e.target.parentNode.querySelector('input').value;
        let matchedArray = [];

        // iterate through data to find matches
        for (const entry in this.list) {
          let game = this.list[entry];
          for (const key in game) {
            let infoValue = game[key].toString();
            // find eventual exact matches
            if (infoValue.includes(searchTerm) && searchTerm !== '') {
              matchedArray.push(game['id']);
            }
          }
        }
        
        for (let i = 0; i < matchedArray.length; i++) {
          const searchResultId = matchedArray[i];
          this.fetchSingleData(i, searchResultId);
        }
      },

      // WIP logs single games SEARCH RESULTS => ONLU FOR API CALL DEMO
      async fetchSingleData(i, id) {
        
        if (id == undefined) {
          console.log('NO REULTS FOUND')
        } else {
          const url = `${this.API._BASEURL}/data/retrieve/${id}?${this.API._VERSION}`;
          let singleResult = await (await fetch(url, {
              "method": "GET"
            })).json().then(data => {return data})

          console.log(singleResult)

          this.resultList.push(singleResult);
        }

        // TO DO: update gamelist with search results
        this.$emit('searchList', this.resultList)
      }

    }
  }
</script>
