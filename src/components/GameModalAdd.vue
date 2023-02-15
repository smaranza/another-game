<template>
    <div class="modal fade" id="addModal" tabindex="-1" aria-hidden="true" data-bs-backdrop="static">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h3 class="modal-title">New Game</h3>
                    <!-- <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button> -->
                </div>
                <div class="modal-body">
                    <form id="entry"
                          class="fancy-validate"
                          :action="modalAction"
                          novalidate
                          @submit="fancyValidate"
                          method="post">

                          <!-- TITLE (label: string)-->
                        <div class="mb-3">
                            <label for="entry--title" class="col-form-label">Title</label>
                            <input type="text" name="label" class="form-control" id="entry--title" placeholder="Game title here.." v-model="gameData.label" required>
                        </div>

                        <!--  DESCRIPTION (description: string)-->
                        <div class="mb-3">
                            <label for="entry--desc" class="col-form-label">Description</label>
                            <input type="textarea" class="form-control" id="entry--desc" name="description"
                                placeholder="Describe the game here..." v-model="gameData.description" required>
                        </div>
                        
                        <!--  RELEASE DATE (date: date) -->
                        <div class="mb-3">
                            <label for="entry--date" class="col-form-label">Release Date</label>
                            <input type="date" class="form-control" id="entry--date" name="date" v-model="gameData.date" required>
                        </div>

                        <!--  COLOR (color: enum)-->
                        <div class="mb-3">
                            <span>Genre</span>
                            <div v-for="(color, index) in colors" class="form-check" :key="index">
                                <input type="radio" class="form-check-input" name="color" :id="color.toLowerCase()" v-model="gameData.color" :value="color" :checked=" index == 0" required>
                                <label :for="color.toLowerCase()" class="form-check-label">{{ color }}</label>
                            </div>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="submit" form="entry" class="btn btn-primary">Add Game</button>
                    <button type="button" class="btn btn-outline-danger" data-bs-dismiss="modal">Cancel</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'GameModalAdd',
    inject: ['API'],
    props: {
        initiator : String
    },
    date() {
        return {
            colors: [],
            gameData: {}
        }
    },
    created() {
        this.colors = this.API._schema.colors;
        this.gameData = {
                label: null,
                description: null,
                date: null,
                color: 'None'
            }
    },
    methods: {
        // POST new game
        async createData(e) {
            const requestOptions = {
                method: "POST",
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(this.gameData)
            };

            await fetch(
                `${this.API._baseurl}/data/create?${this.API._version}`,
                requestOptions
            ).then(response => {
                if (response.ok) {
                    this.closeModal('#addModal')
                }
            });
        },

        // MODAL & FORM handling
        fancyValidate(e) {
            e.preventDefault();
            const form = e.target;
            
            if (!form.checkValidity()) {
                e.stopPropagation()
            } else {
                this.createData();
            }

            form.classList.add('was-validated'); // validation class added here for simplification
        },
        
        resetForm(form) {
            form.reset();
            form.classList.remove('was-validated');
        },

        closeModal(selector) {
            let modalEl = document.querySelector(selector);
            let modalClose = modalEl.querySelector(`[data-bs-dismiss="modal"]`);
            const event = new Event('click');

            modalClose.dispatchEvent(event); // TBD trigger modal close via button clicking
            this.resetForm(modalEl.querySelector(`form`)); // reset form inputs
            this.$emit('closed-modal'); //trigger table update on parent component
        }
    }
}
</script>