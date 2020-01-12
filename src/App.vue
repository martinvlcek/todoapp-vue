<template>
    <div id="app">

        <!-- ADD TODO MODAL CONTENT -->
        <section>
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
                            <button class="button" type="button" @click="isAddTodoModalActive = false">Close</button>
                            <button class="button is-primary" @click.prevent="addNewTodo()">Add todo</button>
                        </footer>
                    </div>
                </form>
            </b-modal>
        </section>

        <!-- EDIT TODO MODAL CONTENT -->
        <section>
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
                            <button class="button" type="button" @click="isEditTodoModalActive = false">Close</button>
                            <button class="button is-primary" @click.prevent="editTodo()">Edit todo</button>
                        </footer>
                    </div>
                </form>
            </b-modal>
        </section>

        <!-- DELETE TODO MODAL CONTENT -->
        <section>
            <b-modal :active.sync="isDeleteTodoModalActive" :width="640" scroll="keep">
                <form action="">
                    <div class="modal-card" style="width: auto">
                        <header class="modal-card-head">
                            <p class="modal-card-title">Delete todo</p>
                        </header>

                        <section class="modal-card-body">
                            <b-field label="Are you sure you want to delete the todo?">
                                <b-input maxlength="200" type="textarea" disabled v-model="deleteTodoModalValue"></b-input>
                            </b-field>
                        </section>
                        
                        <footer class="modal-card-foot">
                            <button class="button" type="button" @click="isDeleteTodoModalActive = false">Close</button>
                            <button class="button is-danger" @click.prevent="deleteTodo()">Delete todo</button>
                        </footer>
                    </div>
                </form>
            </b-modal>
        </section>

        <section class="hero is-dark is-bold">
            <div class="hero-body">
                <div class="container">
                    <h1 class="title">
                        Todoapp
                    </h1>
                    <h2 class="subtitle">
                        created by Martin Vlcek
                    </h2>
                </div>
            </div>
        </section>

        <nav class="panel">
            <p class="panel-heading">
                Todo list
            </p>
            <div class="panel-block">
                <p class="control has-icons-left">
                    <input class="input" type="text" placeholder="Search">
                    <span class="icon is-left">
                        <i class="fas fa-search" aria-hidden="true"></i>
                    </span>
                </p>

                <button class="button is-info add-todo-btn"
                    @click="openAddTodoModal">
                    <b-icon icon="plus-circle-outline"></b-icon>
                    <span>Add todo</span>
                </button>

            </div>
            <p class="panel-tabs">
                <a class="is-active">All</a>
                <a>Completed</a>
                <a>Uncompleted</a>
            </p>
            <a class="panel-block" v-for="todo in allTodos" :key="todo.id">
                <div class="left-block">
                    <div class="field">
                        <b-checkbox :value="true" v-model="todo.isCompleted"
                        type="is-success">
                        </b-checkbox>
                    </div>
                    <p class="todo-text" :class="{ 'todo-done' : todo.isCompleted }">
                        {{todo.value}}
                    </p>
                </div>
    
                <div class="action-icons-block">
                    <span @click="openEditTodoModal(todo)">
                        <b-icon
                            icon="pencil-outline"
                            type="is-primary">
                        </b-icon>
                    </span>

                    <span @click="openDeleteTodoModal(todo)">
                        <b-icon
                            icon="trash-can-outline"
                            type="is-danger">
                        </b-icon>
                    </span>
                    
                </div>
            </a>
        </nav>

    </div>
</template>

<script>

export default {
    data() {
        return {
            addNewTodoValue: '',
            isAddTodoModalActive: false,
            isEditTodoModalActive: false,
            isDeleteTodoModalActive: false,
            deleteTodoModalValue: '',
            editTodoModalValue: '',
            isCompleted: false,
            selectedTodo: null,
            allTodos: [],
            resetId: 1,
            initialTodos: [
                { id: 1, value: 'First todo', isCompleted: true},
                { id: 2, value: 'Second todo', isCompleted: false},
                { id: 3, value: 'Third todo', isCompleted: true},
            ]
        }
    },
    mounted() {
        if (localStorage.getItem("todos")) {
            console.log('1');
            this.allTodos = JSON.parse(localStorage.getItem("todos"));
        } else {
            console.log('2');
            this.allTodos = this.initialTodos;
       }
    },
    watch: {
        allTodos: {
            handler() {
                localStorage.setItem('todos', JSON.stringify(this.allTodos));
            },
            deep: true,
        }
    },
    methods: {
        openAddTodoModal() {
            this.isAddTodoModalActive = true;
            setTimeout(() => {
                this.$refs.newTodoText.focus();
            }, 10);
        },
        addNewTodo() {
            const highestId = Math.max.apply(Math, this.allTodos.map(todo => todo.id));
            const test = (highestId != '-Infinity') ? highestId + 1 : this.resetId;
            this.allTodos.push({id: test, value: this.addNewTodoValue, isCompleted: false});
            this.isAddTodoModalActive = false;
            this.addNewTodoValue = '';
        },
        openDeleteTodoModal(todo) {
            this.selectedTodo = todo;
            this.deleteTodoModalValue = todo.value;
            this.isDeleteTodoModalActive = true
        },
        deleteTodo() {
            this.isDeleteTodoModalActive = false;
            let index = this.allTodos.indexOf(this.selectedTodo);
            // this.allTodos.splice(index, 1);
            this.$delete(this.allTodos, index);
        },
        openEditTodoModal(todo) {
            this.selectedTodo = todo;
            this.editTodoModalValue = todo.value;
            this.isEditTodoModalActive = true;
            setTimeout(() => {
                this.$refs.newEditText.focus();
            }, 10)
        },
        editTodo() {
            this.selectedTodo.value = this.editTodoModalValue;
            this.isEditTodoModalActive = false;
        }
    }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
//   text-align: center;
  color: #2c3e50;
}

.hero {
    text-align: center;
}

.panel {
    max-width: 700px;
    margin: 50px auto;
    .panel-heading {
        text-align: center;
    }
}

.panel-block {
    transition: all .2s ease-in-out;
    &:hover {
        border-left-color: #7957d5;
    }
    .left-block {
        display: inline-flex;
        align-items: center;
        .todo-text {
            font-size: 20px;
        }
    }
    justify-content: space-between !important;
    .field {
        margin-bottom: 0 !important;
    }
    .b-checkbox.checkbox {
        display: flex;
    }
}

.add-todo-btn {
    margin-left: 10px;
}

.action-icons-block {
    .icon {
        transition: transform .2s ease-in-out;
        margin-left: 5px;
        &:hover {
            transform: scale(1.25);
        }
    }
}

.todo-done {
    text-decoration: line-through;
    opacity: .5;
}

</style>
