<template>
  <li class="align-items-center border-bottom" :class="mode == 'grid' ? 'card' : 'row mx-0'">
    <div class="col-1">{{ game.id }}</div>
    <div class="col-2">{{ game.label }}</div>
    <div class="col text-truncate">{{ game.description }}</div>
    <div class="col-auto font-monospace">{{ releaseDate }}</div>
    <div class="col-1">
      <span class="badge w-auto mx-1" :class="gameColorCls">{{ game.color }}</span>
    </div>
    <div class="col-auto hstack">
      <i v-if="mode == 'list'" class="btn  bi-info-square" data-bs-toggle="modal" data-bs-target="#infoModal" @click="setModalContent(game)"> </i>
      <i class="btn bi-pencil-square" data-bs-toggle="modal" data-bs-target="#editModal" @click="setModalContent(game)"></i>
    </div>
  </li>
</template>

<script>
  export default {
    name: 'GameItem',
    props: {
      game: Object,
      mode: String
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
