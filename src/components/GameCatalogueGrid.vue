<template>
    <div>
        <div class="catalogue--header position-relative">
            <div class="catalogue--view btn-group position-absolute end-0" role="group" aria-label="Basic radio toggle button group">
                <input type="radio" 
                       class="btn-check" 
                       name="btnradio"
                       id="list-mode" 
                       autocomplete="off" 
                       checked>
                <label class="btn btn-light" for="list-mode"><i class="bi-list"></i></label>

                <input type="radio" 
                       class="btn-check" 
                       name="btnradio"
                       id="grid-mode" 
                       autocomplete="off">
                <label class="btn btn-light" for="grid-mode"><i class="bi-grid"></i></label>
            </div>
            <div class="list--header"></div>
        </div>
        <ul>
            <GameItem v-for="item in groceryList" :game="item" :key="item.id" />
        </ul>
    </div>
</template>

<script>
import GameItem from './GameItem.vue';

export default {
    name: 'GameCatalogueGrid',
    data() {
        return {
            groceryList: [
                { id: 0, text: 'Vegetables' },
                { id: 1, text: 'Cheese' },
                { id: 2, text: 'Whatever else humans are supposed to eat' }
            ]
        }
    },
    components: {
        GameItem
    }
}
</script>