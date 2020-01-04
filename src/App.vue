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
                                    <b-input v-model="addNewTodoValue" maxlength="200" type="textarea"></b-input>
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
                    @click="isAddTodoModalActive = true">
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
                    <b-icon
                        icon="pencil-outline"
                        type="is-primary">
                    </b-icon>

                    <b-icon
                        icon="trash-can-outline"
                        type="is-danger">
                    </b-icon>
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
            isCompleted: true,
            allTodos: [],
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
    methods: {
        addNewTodo() {
            this.isAddTodoModalActive = false;
            setTimeout (() => {
                this.allTodos.push({id: this.allTodos.length + 1, value: this.addNewTodoValue, isCompleted: false});
                this.saveToLocalStorage();
            }, 500);
        },
        saveToLocalStorage() {
            localStorage.setItem("todos", JSON.stringify(this.allTodos));
            this.allTodos = JSON.parse(localStorage.getItem("todos"));
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
