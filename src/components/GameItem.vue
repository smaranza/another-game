<template>
  <li class="row mx-0 p-2 align-items-center border-bottom">
    <div class="col-1">{{ game.id }}</div>
    <div class="col-3">{{ game.label }}</div>
    <div class="col text-truncate">{{ game.description }}</div>
    <div class="col-auto font-monospace">{{ releaseDate }}</div>
    <div class="badge col-1" :class="gameColorCls">{{ game.color }}</div>
    <div class="col-1">
      <i class="btn bi-info-square" data-bs-toggle="modal" data-bs-target="#infoModal" @click="setModalContent(game)"> </i>
      <i class="btn bi-pencil-square" data-bs-toggle="modal" data-bs-target="#editModal" @click="setModalContent(game)"></i>
    </div>
  </li>
</template>

<script>
  export default {
    name: 'GameItem',
    props: {
      game: Object
    },
    data(){
      return {
        releaseDate : this.formatDate(this.game.date),
        gameColorCls : `bg-${this.game.color.toLowerCase()}`
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
