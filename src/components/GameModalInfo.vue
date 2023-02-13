<template>
  <div class="modal fade" id="infoModal" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered ">
      <div class="modal-content">
        <div class="modal-header">
          <h3 class="modal-title fs-5" id="exampleModalLabel">{{ content.label }}</h3>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          <!-- <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button> -->
        </div>
        <div class="modal-body">
          <div class="">{{ content.description }}</div>
        </div>
        <div class="modal-footer justify-content-between">
          <span class="badge" :class="content.gameColorCls">{{ content.color }}</span>
          <div class=""><span class="fw-bold">Release Date:</span> {{ content.date }}</div>
          <!-- <div class="badge col-1" :class="gameColorCls">{{ content.color }}</div> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GameModalInfo',
  props: {
    content: Object
  }
}
</script>