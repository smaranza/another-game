<template>
  <!-- LIST view -->

  <li v-if="!isGridMode" class="align-items-center row mx-0 border-bottom py-2">
    <div class="col-1">{{ game.id }}</div>
    <div class="col-2 text-capitalize">{{ game.label }}</div>
    <div class="col text-truncate">{{ game.description }}</div>
    <div class="col-1">{{ releaseDate }}</div>
    <div class="col-1">
      <span class="badge w-auto mx-1" :class="gameColorCls">{{ game.color }}</span>
    </div>
    <div class="col-1 hstack'">
      <i class="btn bi-info-square" data-bs-toggle="modal" data-bs-target="#infoModal" @click="setModalContent(game)"> </i>
      <i class="btn bi-pencil-square" data-bs-toggle="modal" data-bs-target="#addModal" @click="setModalContent(game)"></i>
    </div>
  </li>

  <!-- GRID view -->
  <div v-else class="col-4 py-3">
    <div class="card h-100 shadow another-card ">
      <div class="card-header hstack align-items-end">
        <h3 class="card-title my-2 text-capitalize">{{ game.label }} <small class="text-muted fs-6 fst-italic">#{{ game.id }}</small></h3>
        <i class="btn bi-pencil-square ms-auto fs-5 text-muted" data-bs-toggle="modal" data-bs-target="#addModal" @click="setModalContent(game)"></i>
      </div>
      <div class="card-body">
        <p>{{ game.description }}</p>
      </div>
      <div class="card-footer hstack justify-content-between py-3">
        <div class="badge w-auto mx-1" :class="gameColorCls">{{ game.color }}</div>
        <div>Released on: <span class="font-monospace">{{ releaseDate }}</span></div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'GameItem',
    props: {
      game: Object,
      mode: String
    },
    computed: {
      gameColorCls() {
        return`bg-${this.game.color.toLowerCase()}`
      },
      releaseDate() {
        return this.formatDate(this.game.date)
      },
      isGridMode() {
        return this.mode == 'grid'
      }
    },
    methods: {
      formatDate(stringData) {
        return new Date(stringData).toLocaleDateString(undefined, {
          day: '2-digit',
          month: '2-digit',
          year: 'numeric'
        });
      },
      setModalContent(thisGame) {
        thisGame.date = this.releaseDate;
        thisGame.gameColorCls = this.gameColorCls;
        this.$emit('select-game', thisGame);
      }
    }
  }
</script>
