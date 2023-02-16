<template>
  <header class="d-flex flex-md-row flex-column navbar mt-3 mt-md-5">
    <div class="d-flex flex-md-row flex-column w-100 align-items-center justify-content-between">
      <h1>Games Catalogue</h1>
      <button class="btn btn-blue align-self-center" data-bs-toggle="modal" data-bs-target="#addModal" @click="setModalAddType"><i class="bi-plus"></i> Add new</button>
    </div>
      
      <!--  Switch List / Grid view -->
      <div class="catalogue--settings d-flex flex-row w-100 justify-content-center justify-content-md-between my-2">
        <p class="lead mb-0 d-none d-md-block"> This is the description of the archive. Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
        <div class="catalogue--view btn-group align-self-start" role="group">
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
  <div class="catalogue--container position-relative row mx-0">
    <!-- Catalogue Header --> 
    <div v-if="viewMode == 'list'" class="catalogue--header fw-bold row mx-0 border-dark align-items-center border-top border-bottom py-2">
      <div class="table--id col-1">#</div>
      <div class="table--title col-2">Title</div>
      <div class="table--description col d-none d-md-block ">Description</div>
      <div class="table--date col-1 d-none d-md-block">Release Date</div>
      <div class="table--tag col-1 offset-3 offset-md-0">Genre</div>
      <div class="table--edit col-1"></div>
    </div>

    <!-- Catalogue Body -->
    <ul class="catalogue--body px-0 mx-0 row">
      <GameItem v-for="(item, index) in gameList" :game="item" :key="index" :mode="viewMode"
        :class="[((index + 1) % 2 == 0 && viewMode == 'list' ? 'bg-gray bg-opacity-10' : '')]" 
        @select-game="setModalGameData"/>
    </ul>
  </div>
  
  <!-- Modals -->
  <ModalInfo id="infoModal" :content="selectedGame"/>
  <ModalAddEdit :isEdit="isEdit" @closed-modal="fetchAllData" :content="selectedGame"/>
</template>

<script>
  import GameItem from './AnotherCatalogue/GameItem.vue';
  import ModalInfo from './AnotherCatalogue/GameModal/ModalInfo.vue';
  import ModalAddEdit from './AnotherCatalogue/GameModal/ModalAddEdit.vue';
  
  export default {
    name: 'GameCatalogueGrid',
    inject: ['API'],
    components: {
      GameItem,
      ModalInfo,
      ModalAddEdit,
    },
    data() {
      return {
        viewMode: "list",
        gameList: [],
        selectedGame: '',
        isEdit: false
      }
    },

    created() {
      this.fetchAllData()
    },

    methods: {
      // GET all games
      async fetchAllData() {
        const url = `${this.API._BASEURL}/data/retrieve/all?${this.API._VERSION}`;
        this.gameList = await (await fetch(url, {
          "method": "GET"
        })).json();

        // emit full game list to be used by search
        this.$emit('listUpdate', this.gameList);
      },

      setModalGameData(gameData) {
        this.selectedGame = gameData;
        this.isEdit = true; 
      },

      setModalAddType() {
        this.selectedGame = { color: "None" };
        this.isEdit = false // is add
      }
    }
  }
</script>