<template>
  <header class="d-flex navbar">
      <h1>Games Catalogue</h1>
      <button class="btn btn-outline-blue" data-bs-toggle="modal" data-bs-target="#addModal"><i class="bi-plus"></i> Add new</button>
      
      <!--  Switch List / Grid view -->
      <div class="catalogue--settings d-inline-flex position-relative w-100 justify-content-end mb-2">
        <div class="catalogue--view btn-group" role="group">
          <input type="radio" class="btn-check" name="btnradio" id="list-mode" value="list" autocomplete="off"
            v-model="viewMode">
          <label class="btn btn-light" for="list-mode"><i class="bi-list"></i></label>
    
          <input type="radio" class="btn-check" name="btnradio" id="grid-mode" value="grid" autocomplete="off"
            v-model="viewMode">
          <label class="btn btn-light" for="grid-mode"><i class="bi-grid"></i></label>
        </div>
      </div>
  </header>

  <!-- Game Catalogue  -->
  <div class="catalogue--container position-relative">
    <!-- Catalogue Header --> 
    <div v-if="viewMode == 'list'" class="catalogue--header row mx-0 border-dark align-items-center border-top border-bottom">
      <div class="table--id col-1">#</div>
      <div class="table--title col-2">Title</div>
      <div class="table--description col">Description</div>
      <div class="table--date col-auto">Release Date</div>
      <div class="table--tag col-1">Genre</div>
      <div class="table--edit col-auto"></div>
    </div>

    <!-- Catalogue Body -->
    <ul class="catalogue--body px-0 mx-0 row">
      <GameItem v-for="(item, index) in gameList" :game="item" :key="index" :mode="viewMode"
        :class="[(viewMode == 'list' ? 'col-12' : 'col-6'), ((gameList[item] + 1) % 2 == 0 ? 'bg-gray bg-opacity-10' : '')]" 
        @select-game="setModalGame"/>
    </ul>
  </div>
  
  <!-- Modals -->
  <GameModalInfo :content="selectedGame"/>
  <GameModalEdit :content="selectedGame"/>
  <GameModalAdd :initiator="'create'" @closed-modal="fetchAllData"/>
</template>

<script>
  import GameItem from './GameItem.vue';
  import GameModalInfo from './GameModalInfo.vue';
  import GameModalEdit from './GameModalEdit.vue';
  import GameModalAdd from './GameModalAdd.vue';
  
  export default {
    name: 'GameCatalogueGrid',
    inject: ['API'],
    components: {
      GameItem,
      GameModalInfo,
      GameModalEdit,
      GameModalAdd
    },
    data() {
      return {
        viewMode: "list",
        gameList: [],
        selectedGame: ''
      }
    },
    mounted() {
      this.fetchAllData()
    },
    methods: {
      async fetchAllData() {
        const url = `${this.API._baseurl}/data/retrieve/all?${this.API._version}`;
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