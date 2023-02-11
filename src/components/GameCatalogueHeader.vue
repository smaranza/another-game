<template>
    <header class="d-flex navbar">
        <h1>Games Catalogue</h1>
        <button class="btn btn-primary"><i class="bi-plus"></i> Add new</button>
    </header>
</template>

<script>
  export default {
    name: 'GameCatalogueHeader',
    props: {
      user: String
    }
  }
</script>