<template>
  <div class="catalogue--settings d-inline-flex position-relative w-100 justify-content-end mb-2">
    <!--  Switch List / Grid view -->
    <div class="catalogue--view btn-group" role="group">
      <input type="radio" class="btn-check" name="btnradio" id="list-mode" value="list-mode" autocomplete="off"
        v-model="view">
      <label class="btn btn-light" for="list-mode"><i class="bi-list"></i></label>

      <input type="radio" class="btn-check" name="btnradio" id="grid-mode" value="grid-mode" autocomplete="off"
        v-model="view">
      <label class="btn btn-light" for="grid-mode"><i class="bi-grid"></i></label>
    </div>
  </div>

  <!-- Game Catalogue  -->
  <div class="catalogue--container position-relative">
    <!-- Catalogue Header -->
    <div class="catalogue--header row mx-0 border-dark align-items-center border-top border-bottom">
      <div class="table--id col-1">#</div>
      <div class="table--title col-3">Title</div>
      <div class="table--description col">Description</div>
      <div class="table--date col-auto">Release Date</div>
      <div class="table--tag col-1">Genre</div>
      <div class="table--edit col-1">Edit</div>
    </div>

    <!-- Catalogue Body -->
    <ul class="catalogue--body px-0 mx-0 row">
      <GameItem v-for="(item, index) in gameList" :game="item" :key="index"
        :class="[(view == 'list-mode' ? 'col-12' : 'col-6'), ((index + 1) % 2 == 0 ? 'bg-gray bg-opacity-10' : '')]" 
        @select-game="setModalGame"/>
    </ul>
  </div>

  <GameModalInfo :content="selectedGame"/>
  <GameModalEdit :content="selectedGame"/>
</template>

<script>
  import GameItem from './GameItem.vue';
  import GameModalInfo from './GameModalInfo.vue';
  import GameModalEdit from './GameModalEdit.vue';
  
  export default {
    name: 'GameCatalogueGrid',
    inject: ['API'],
    components: {
      GameItem,
      GameModalInfo,
      GameModalEdit
    },
    data() {
      return {
        view: "list-mode",
        gameList: [],
        selectedGame: ''
      }
    },
    mounted() {
      this.fetchAllData()
    },
    methods: {
      async fetchAllData() {
        const url = `${this.API._BASEURL}/data/retrieve/all?${this.API._VERSION}`;
        this.gameList = await (await fetch(url, {
          "method": "GET"
        })).json();
      },
      setModalGame(gameData) {
        console.log(gameData);
        this.selectedGame = gameData;
      }
    }
  }
</script>