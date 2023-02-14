<template>
    <div class="container">
        <!--  TODO CLEANUP -->
        <!-- <GameCatalogueHeader /> -->
        <GameCatalogueGrid />
    </div>
</template>

<script>
    import GameCatalogueHeader from './GameCatalogueHeader.vue'
    import GameCatalogueGrid from './GameCatalogueGrid.vue'

  export default {
    name: 'AnotherCatalogue',
    components: {
        // TODO CLEANUP
        // GameCatalogueHeader,
        GameCatalogueGrid,
    },
    props: {
      user: String
    }
  }
</script>
