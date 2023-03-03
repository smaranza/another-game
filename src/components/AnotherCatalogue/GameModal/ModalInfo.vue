<template>
  <div class="modal fade" id="infoModal" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered ">
      <div class="modal-content">
        <div class="modal-header">
          <h3 class="modal-title">{{ content.label }}</h3>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <p class="">{{ content.description }}</p>
        </div>
        <div class="modal-footer justify-content-between">
          <span class="badge" :class="content.gameColorCls">{{ content.color }}</span>
          <div class=""><span class="fw-bold">Release Date:</span> {{ releaseDate }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ModalInfo',
  props: {
    content: Object
  },

  methods: {
    formatDate(stringDate) {
      return new Date(stringDate).toLocaleDateString(undefined, {
        day: '2-digit',
        month: '2-digit',
        year: 'numeric'
      });
    }
  },

  computed: {
    releaseDate() {
      return this.formatDate(this.content.date)
    }
  }
}
</script>