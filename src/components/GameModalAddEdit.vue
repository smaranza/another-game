<template>
    <div class="modal fade" id="addModal" tabindex="-1" aria-hidden="true" data-bs-backdrop="static">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h3 class="modal-title">{{ isEdit ? "Edit Game" : "New Game" }}</h3>
                    <!-- <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button> -->
                </div>
                <div class="modal-body">
                    <form id="entry"
                          class="fancy-validate"
                          novalidate
                          @submit="dataRequest"
                    >
                        <!-- <input v-if="isEdit" type="hidden" name="id" :value="content.id"> -->

                        <!-- TITLE (label: string)-->
                        <div class="mb-3">
                            <label for="entry--title" class="col-form-label">Title</label>
                            <input type="text" name="label" class="form-control" id="entry--title" placeholder="Game title here.." v-model="gameData.label" required>
                        </div>

                        <!--  DESCRIPTION (description: string)-->
                        <div class="mb-3">
                            <label for="entry--desc" class="col-form-label">Description</label>
                            <textarea class="form-control" id="entry--desc" name="description"
                                placeholder="Describe the game here..." v-model="gameData.description"  required></textarea>
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
                                <input type="radio" class="form-check-input" name="color" :id="color.toLowerCase()" v-model="gameData.color" :checked=" color == gameData.color" required>
                                <label :for="color.toLowerCase()" class="form-check-label">{{ color }}</label>
                            </div>
                        </div>
                    </form>
                </div>
                <div class="modal-footer justify-content-between">
                    <button v-if="isEdit" type="submit" class="btn btn-outline-danger"><i class="bi-trash"></i> Delete Game</button>
                    <div class="ms-auto">
                        <button type="button" class="btn btn-outline-secondary me-2" data-bs-dismiss="modal" @click="clearForm">Cancel</button>
                        <button type="submit" form="entry" class="btn btn-primary ml-2">{{ isEdit ? "Save Changes" : "Save" }}</button>
                    </div>
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
        content: Object,
        isEdit: Boolean
    },
    date() {
        return {
            colors: [],
            dateValue: ''
            // gameData: {},
        }
    },
    created() {
        this.colors = this.API._schema.colors;
        this.dateValue = this.formatDateValue()
    },
    computed: {
        gameData() {
            // format date as valid <input type="date"> value
            let dateValue = this.content.date;
            if (this.content.date !== undefined) {
                let dataSplit = this.content.date.split('/');
                dataSplit.reverse();
                dateValue = dataSplit.join('-');
            }

            //return game data
            return {
                label: this.content.label,
                description: this.content.description,
                date: dateValue,
                color: this.content.color || 'None'
            }
        }
    },
    methods: {
        // handle request POST / update
        dataRequest(e) {
            this.isValidForm = false;
            this.fancyValidate(e)

            if (this.isValidForm == true) {
                if (this.isEdit) {
                    this.updateData();
                } else {
                    this.createData();
                }
            }
        },

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

        // PUT update game
        async updateData(e) {
            this.gameData.id = this.content.id;

            console.log(this.gameData);

            const requestOptions = {
                method: "PUT",
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(this.gameData)
            };

            // await fetch(
            //     `${this.API._baseurl}/data/update?${this.API._version}`,
            //     requestOptions
            // ).then(response => {
            //     if (response.ok) {
            //         this.closeModal('#addModal')
            //     }
            // });
        },

        // validation on form to prevent submit
        fancyValidate(e) {
            e.preventDefault();
            const form = e.target;
            
            if (!form.checkValidity()) {
                e.stopPropagation();
            } else {
                this.isValidForm = true;
            }

            form.classList.add('was-validated'); // validation class added here for simplification
        },
        
        // clear form inputs and validation for next modal open
        clearForm() {
            const form = document.querySelector('.modal form');
            form.reset();
            form.classList.remove('was-validated');
        },

        // manually trigger modal close on success, and clear form / validation
        closeModal(selector) {
            let modalEl = document.querySelector(selector);
            let modalClose = modalEl.querySelector(`[data-bs-dismiss="modal"]`);
            const event = new Event('click');

            modalClose.dispatchEvent(event); // TBD trigger modal close via button clicking
            this.clearForm(); // reset form inputs
            this.$emit('closed-modal'); //trigger table update on parent component
        },

        formatDateValue() {
            this.dateValue = this.content.date;

            if (this.content.date !== undefined) {
                console.log(this.content.date);
                this.dateValue = this.content.date.replace('/', '-')
            }
            return this.dateValue
        }
    }
}
</script>