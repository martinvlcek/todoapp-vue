<template>
    <!-- <section> -->
        <b-modal :active.sync="isAddTodoModalActive" :width="640" scroll="keep">
            <form action="">
                <div class="modal-card" style="width: auto">
                    <header class="modal-card-head">
                        <p class="modal-card-title">Add new todo</p>
                    </header>
                    
                    <section class="modal-card-body">
                        <b-field label="Todo message">
                            <b-input v-model="addNewTodoValue" ref="newTodoText" maxlength="200" type="textarea"></b-input>
                        </b-field>
                    </section>

                    <footer class="modal-card-foot">
                        <button class="button" type="button" @click="closeAddNewTodoModal()">Close</button>
                        <button class="button is-primary" @click.prevent="addNewTodo()">Add todo</button>
                    </footer>
                </div>
            </form>
        </b-modal>
    <!-- </section> -->
</template>

<script>

export default {
    props: [
        'isAddTodoModalActive',
        'allTodos'
    ],
    data() {
        return {
            resetId: 1,
            addNewTodoValue: '',
            isAddTodoModalShown: this.isAddTodoModalActive
        }
    },
    methods: {
        closeAddNewTodoModal() {
            this.isAddTodoModalShown = false;
            this.$emit('closeModal', this.isAddTodoModalShown);
        },
        addNewTodo() {
            let highestId = Math.max.apply(Math, this.allTodos.map(todo => todo.id));
            let lastId = (highestId != '-Infinity') ? highestId + 1 : this.resetId;
            this.allTodos.push({id: lastId, value: this.addNewTodoValue, isCompleted: false});
            this.isAddTodoModalShown = false;
            this.addNewTodoValue = '';
            this.$emit('closeModal', false);
        }
    }
}
</script>