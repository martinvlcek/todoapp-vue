<template>
    <div class="todo-body">

        <edit-todo-modal
            :isEditTodoModalActive="isEditTodoModalActive"
            :editTodoModalValue="editTodoModalValue"
            :selectedTodo="selectedTodo"
            @closeModal="isEditTodoModalActive = $event"
        />

        <delete-todo-modal
            :isDeleteTodoModalActive="isDeleteTodoModalActive"
            :deleteTodoModalValue="deleteTodoModalValue"
            :selectedTodo="selectedTodo"
            :allTodos="allTodos"
            @closeModal="isDeleteTodoModalActive = $event"
        />
        
        <transition-group name="todos" tag="a">
            <a class="panel-block" v-for="todo in filteredTodos" :key="todo.id">
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
        </transition-group>
    </div>
</template>

<script>
import EditTodoModal from './modals/EditTodoModal'
import DeleteTodoModal from './modals/DeleteTodoModal'

let filters = {
    all(allTodos) {
        return allTodos;
    },
    completed(allTodos) {
        return allTodos.filter(function(todo) {
            return todo.isCompleted;
        })
    },
    active(allTodos) {
        return allTodos.filter(function(todo) {
            return !todo.isCompleted;
        }) 
    }
};

export default {
    props: ['visibleTodosNext', 'search'],
    components: {
        EditTodoModal,
        DeleteTodoModal
    },
    data() {
        return {
            isEditTodoModalActive: false,
            selectedTodo: null,
            editTodoModalValue: '',
            isDeleteTodoModalActive: false,
            deleteTodoModalValue: '',
            initialTodos: [
                { id: 1, value: 'First todo', isCompleted: true},
                { id: 2, value: 'Second todo', isCompleted: false},
                { id: 3, value: 'Third todo', isCompleted: true},
                { id: 4, value: 'Fourth todo', isCompleted: false},
                { id: 5, value: 'Fifth todo', isCompleted: true }
            ],
            allTodos: []
        }
    },
    mounted() {
        if (localStorage.getItem("todos")) {
            this.allTodos = JSON.parse(localStorage.getItem("todos"));
        } else {
            this.allTodos = this.initialTodos;
       }
       this.$emit('getAllTodos', this.allTodos);
    },
    computed: {
        filteredTodos() {
            let filtered = filters[this.visibleTodosNext](this.allTodos);

            if (this.search) {
                filtered = filtered.filter(
                    m => m.value.toLowerCase().indexOf(this.search.toLowerCase()) !== -1
                );
            }
            return filtered;
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
        openEditTodoModal(todo) {
            this.selectedTodo = todo;
            this.editTodoModalValue = todo.value;
            this.isEditTodoModalActive = true;
        },
        openDeleteTodoModal(todo) {
            this.selectedTodo = todo;
            this.deleteTodoModalValue = todo.value;
            this.isDeleteTodoModalActive = true
        }
    }
}
</script>

<style lang="scss">
.panel {
    .todo-body {
        .panel-block {
            transition: opacity .5s ease-in-out, transform .5s ease-in-out;
            &:first-child {
                border-top: none;
            }
            &:hover {
                border-left-color: #7957d5;
                .action-icons-block {
                    opacity: 1;
                    right: 0;
                }
                .field {
                    opacity: 1;
                    left: 0;
                }
            }
            .left-block {
                display: inline-flex;
                align-items: center;
                .todo-text {
                    font-size: 20px;
                }
                .todo-done {
                    text-decoration: line-through;
                    opacity: .5;
                }
            }
            justify-content: space-between !important;
            .action-icons-block {
                opacity: .5;
                transition: opacity .2s ease-in-out;
                .icon {
                    transition: transform .2s ease-in-out;
                    margin-left: 5px;
                    &:hover {
                        transform: scale(1.25);
                    }
                }
            }
            .field {
                margin-bottom: 0 !important;
                opacity: .5;
                transition: opacity .2s ease-in-out;
            }
            .b-checkbox.checkbox {
                display: flex;
            }
        }
    }
}

.todos-enter, .todos-leave-to  {
    opacity: 0;
    // transform: translateY(25px);
    transform: scale(.90);
}
</style>