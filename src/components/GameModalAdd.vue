<template>
    <div class="modal fade" id="addModal" tabindex="-1" aria-hidden="true" data-bs-backdrop="static">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h3 class="modal-title" id="exampleModalLabel">New Game</h3>
                    <!-- <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button> -->
                </div>
                <div class="modal-body">
                    <!-- <div class="">{{ content.description }}</div> -->
                    <form id="entry" 
                          class="fancy-validate" 
                          novalidate
                          @submit="fancyValidate">
                        <div class="mb-3">
                            <label for="entry--title" class="col-form-label">Title</label>
                            <input type="text" name="label" class="form-control" id="entry--title" placeholder="Game title here.." required>
                        </div>
                        <div class="mb-3">
                            <label for="entry--desc" class="col-form-label">Description</label>
                            <input type="textarea" class="form-control" id="entry--desc"
                                placeholder="Describe the game here..." required>
                        </div>
                        <div class="mb-3">
                            <label for="entry--date" class="col-form-label">Release Date</label>
                            <input type="date" class="form-control" id="entry--date" :value="now" required>
                        </div>
                        <div class="mb-3">
                            <span>Genre</span>
                            <!--  radio for color schema -->
                            <div v-for="(color, index) in colors" class="form-check" :key="index">
                                <input type="radio" class="form-check-input" name="color" :id="color" :value="color" :checked=" index == 0" required>
                                <label :for="color" class="form-check-label">{{ color }}</label>
                            </div>
                        </div>

                        <label for="validationCustom01" class="form-label">First name</label>
                        <input type="text" class="form-control" id="validationCustom01" required>
                        <div class="valid-feedback">
                        Looks good!
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="submit" form="entry" class="btn btn-primary">Add Game</button>
                    <button type="button" class="btn btn-outline-danger" data-bs-dismiss="modal" @click="resetForm('.fancy-validate')">Cancel</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'GameModalAdd',
    inject: ['API'],
    date() {
        return {
            now: null,
            colors: [],
            isValidated: false,
            validateClass: ''
        }
    },
    created() {
        this.getTodayValue();
        this.colors = this.API._schema.colors;
        // this.isValidated = false;
    },
    methods: {
        getTodayValue() {
            this.now = new Date().toISOString().substring(0,10);
        },
        fancyValidate(e) {
            let form = e.target;
            if (!form.checkValidity()) {
                e.preventDefault()
                e.stopPropagation()
            }
            form.classList.add('was-validated'); // validation class added here for simplification
        },
        resetForm(selector) {
            let form = document.querySelector(selector);
            form.reset();
            form.classList.remove('was-validated');
        }
    }
}
</script>