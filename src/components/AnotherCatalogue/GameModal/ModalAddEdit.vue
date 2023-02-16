<template>
    <div class="modal fade" id="addModal" tabindex="-1" aria-hidden="true" data-bs-backdrop="static">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h3 class="modal-title">{{ isEdit ? "Edit Game" : "New Game" }}</h3>
                    <button v-if="isEdit" type="button" class="btn btn-outline-danger d-md-none" data-bs-toggle="modal" data-bs-target="#confirmModal"><i class="bi-trash"></i> Delete Game</button>
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
                            <label for="entry--title" class="col-form-label h6">Title</label>
                            <input type="text" name="label" class="form-control" id="entry--title" placeholder="Game title here.." v-model="gameData.label" required>
                        </div>

                        <!--  DESCRIPTION (description: string)-->
                        <div class="mb-3">
                            <label for="entry--desc" class="col-form-label h6">Description</label>
                            <textarea class="form-control" id="entry--desc" name="description"
                                placeholder="Describe the game here..." v-model="gameData.description"  required></textarea>
                        </div>
                        
                        <!--  RELEASE DATE (date: date) -->
                        <div class="mb-3">
                            <label for="entry--date" class="col-form-label h6">Release Date</label>
                            <input type="date" class="form-control" id="entry--date" name="date" v-model="gameData.date" required>
                        </div>

                        <!--  COLOR (color: enum)-->
                        <div class="mb-3">
                            <h6 class="col-form-label">Genre</h6>
                            <div v-for="(color, index) in colors" class="form-check" :key="index">
                                <input type="radio" class="form-check-input" name="color" :id="color.toLowerCase()" v-model="gameData.color" :value="color" :checked="color == gameData.color ? true : false" required>
                                <label :for="color.toLowerCase()" class="form-check-label">{{ color }}</label>
                            </div>
                        </div>
                    </form>
                </div>
                <div class="modal-footer justify-content-between">
                    <button v-if="isEdit" type="button" class="btn btn-outline-danger d-none d-md-block" data-bs-toggle="modal" data-bs-target="#confirmModal"><i class="bi-trash"></i> Delete Game</button>
                    <div class="ms-auto">
                        <button type="button" class="btn btn-outline-secondary me-2" data-bs-dismiss="modal" @click="clearForm">Cancel</button>
                        <button type="submit" form="entry" class="btn btn-primary">{{ isEdit ? "Save Changes" : "Save" }}</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="modal fade" id="confirmModal" tabindex="-1" aria-hidden="true" data-bs-backdrop="static">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content text-center">
                <div class="modal-header text-danger vstack">
                    <i class="bi-exclamation-octagon fs-1"></i> 
                    <h3 class="">Are you sure?</h3>
                </div>
                <div class="modal-body">
                    <p>Do you want to delete <span class="fst-italic fw-bolder">{{gameData.label}}</span>?<br>This action will not be reversible!</p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-link" data-bs-dismiss="modal" >No, I changed my mind</button>
                    <button type="button" class="btn btn-danger" @click="deleteData">Yes, please</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ModalAdd',
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
        createData(e) {
            this.sendAPIRequest('POST', '/data/create?', this.gameData);
        },

        // PUT update game
        updateData(e) {
            this.gameData.id = this.content.id;
            this.sendAPIRequest('PUT', '/data/update?', this.gameData);
        },

        // DELETE delete game
        deleteData(e) {
            this.gameData.id = this.content.id;
            this.sendAPIRequest('DELETE', `/data/delete/${this.gameData.id}?`);
            this.closeModal('#confirmModal')
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

        async sendAPIRequest(method, url, data) {
            const requestOptions = {
                method: method,
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(data)
            };

            await fetch(
                `${this.API._BASEURL}${url}${this.API._VERSION}`,
                requestOptions
            ).then(response => {
                if (response.ok) {
                    this.closeModal('#addModal')
                }
            });
        }

    }
}
</script>