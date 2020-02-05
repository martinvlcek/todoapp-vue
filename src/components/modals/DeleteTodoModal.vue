<template>
    <!-- <section> -->
        <b-modal :active.sync="isDeleteTodoModalActive" :width="640" scroll="keep">
            <form action="">
                <div class="modal-card" style="width: auto">
                    <header class="modal-card-head">
                        <p class="modal-card-title">Delete todo</p>
                    </header>

                    <section class="modal-card-body">
                        <b-field label="Are you sure you want to delete the todo?">
                            <b-input maxlength="200" type="textarea" disabled :value="deleteTodoModalValue"></b-input>
                        </b-field>
                    </section>
                    
                    <footer class="modal-card-foot">
                        <button class="button" type="button" @click="closeModal()">Close</button>
                        <button class="button is-danger" @click.prevent="deleteTodo()">Delete todo</button>
                    </footer>
                </div>
            </form>
        </b-modal>
    <!-- </section> -->
</template>

<script>
export default {
    props: [
        'isDeleteTodoModalActive',
        'deleteTodoModalValue',
        'selectedTodo',
        'allTodos'
    ],
    data() {
        return {
            isDeleteModalShown: this.isDeleteTodoModalActive
        }
    },
    methods: {
        closeModal() {
            this.isDeleteModalShown = false;
            this.$emit('closeModal', this.isDeleteModalShown)
        },
        deleteTodo() {
            this.isDeleteModalShown = false;
            let index = this.allTodos.indexOf(this.selectedTodo);
            // this.allTodos.splice(index, 1);
            this.$delete(this.allTodos, index);
            this.$emit('closeModal', this.isDeleteModalShown)
        },
    }
}
</script>