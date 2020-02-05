<template>
    <!-- <section> -->
        <b-modal :active.sync="isEditTodoModalActive" :width="640" scroll="keep">
            <form action="">
                <div class="modal-card" style="width: auto">
                    <header class="modal-card-head">
                        <p class="modal-card-title">Edit todo</p>
                    </header>
                    
                    <section class="modal-card-body">
                        <b-field label="Todo message">
                            <b-input v-model="editTodoModalValue" ref="newEditText" maxlength="200" type="textarea"></b-input>
                        </b-field>

                    </section>
                    <footer class="modal-card-foot">
                        <button class="button" type="button" @click="closeModal()">Close</button>
                        <button class="button is-primary" @click.prevent="editTodo()">Edit todo</button>
                    </footer>
                </div>
            </form>
        </b-modal>
    <!-- </section> -->
</template>

<script>
export default {
    props: [
        'isEditTodoModalActive',
        'editTodoModalValue',
        'selectedTodo'
    ],
    methods: {
        closeModal() {
            this.$emit('closeModal', false)
        },
        editTodo() {
            this.selectedTodo.value = this.editTodoModalValue;
            this.$emit('closeModal', false)
        }
    }
}
</script>