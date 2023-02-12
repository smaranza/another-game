<template>
    <div class="catalogue--settings d-inline-flex position-relative w-100 justify-content-end mb-2">
        <!--  Switch List / Grid view -->
        <div class="catalogue--view btn-group" role="group" aria-label="Basic radio toggle button group">{{ view }}
            <input type="radio" class="btn-check" name="btnradio" id="list-mode" value="list-mode" autocomplete="off"
                v-model="view">
            <label class="btn btn-light" for="list-mode"><i class="bi-list"></i></label>

            <input type="radio" class="btn-check" name="btnradio" id="grid-mode" value="grid-mode" autocomplete="off"
                v-model="view">
            <label class="btn btn-light" for="grid-mode"><i class="bi-grid"></i></label>
        </div>
    </div>

    <!-- Game Catalogue  -->
    <div class="catalogue--container position-relative">
        <!-- Catalogue Header -->
        <div class="catalogue--header row mx-0">
            <div class="table--id col-1">#</div>
            <div class="table--title col-3">Title</div>
            <div class="table--description col">Description</div>
            <div class="table--date col-1">Release Date</div>
            <div class="table--tag col-1">Genre</div>
            <div class="table--edit col-2">Edit</div>
        </div>
        
        <!-- Catalogue Body -->
        <ul class="catalogue--body px-0 mx-0 row" >
            <GameItem v-for="(item, index) in gameList" :game="item" :key="index" :class="[(view == 'list-mode' ? 'col-12' : 'col-6'), ((index + 1) % 2 == 0 ? 'bg-gray bg-opacity-10' : '')]"/>
        </ul>
    </div>
</template>

<script>
import GameItem from './GameItem.vue';

export default {
    name: 'GameCatalogueGrid',
    data() {
        return {
            view: "list-mode",
            // TODO get all entries, static data dor testing
            gameList: [
                {
                    "id": 1,
                    "label": "Dead Space",
                    "description": "Dead Space è un videogioco di fantascienza survival horror in terza persona sviluppato da Visceral Games al tempo chiamata EA redwood shores",
                    "date": "2008-10-14T00:00:00",
                    "color": "Black"
                },
                {
                    "id": 2,
                    "label": "Super Mario World",
                    "description": "Super Mario World, è un videogioco a piattaforme per Super Nintendo Entertainment System sviluppato da Nintendo Entertainment Analysis & Development",
                    "date": "1990-11-21T00:00:00",
                    "color": "Yellow"
                },
                {
                    "id": 3,
                    "label": "Forspoken",
                    "description": "Forspoken è un videogioco action RPG sviluppato da Luminous Productions e pubblicato da Square Enix per PlayStation 5 e Microsoft Windows",
                    "date": "2023-01-24T00:00:00",
                    "color": "Blue"
                },
                {
                    "id": 5,
                    "label": "Ghost of Tsushima",
                    "description": "Ghost of Tsushima è un videogioco d'avventura dinamica e stealth sviluppato da Sucker Punch Productions e pubblicato da Sony Interactive Entertainment per PlayStation 4",
                    "date": "2020-07-17T00:00:00",
                    "color": "Green"
                },
                {
                    "id": 6,
                    "label": "Assetto Corsa",
                    "description": "Assetto Corsa è un simulatore di guida sviluppato da Kunos Simulazioni. Il gioco è inizialmente uscito attraverso il progetto Early Access di Steam nel 2014",
                    "date": "2014-12-19T00:00:00",
                    "color": "Red"
                }
            ]
        }
    },
    components: {
        GameItem
    }
}
</script>