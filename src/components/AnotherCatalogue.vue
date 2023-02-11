<template>
    <div class="container">
        <GameCatalogueHeader />
        <GameCatalogueGrid />
    </div>
</template>

<script>
    import GameCatalogueHeader from './GameCatalogueHeader.vue'
    import GameCatalogueGrid from './GameCatalogueGrid.vue'

  export default {
    name: 'AnotherCatalogue',
    components: {
        GameCatalogueHeader,
        GameCatalogueGrid,
    },
    props: {
      user: String
    }
  }
</script>
