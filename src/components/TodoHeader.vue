<template>
    <div class="todo-header">

        <add-todo-modal
            :isAddTodoModalActive="isAddTodoModalActive"
            :allTodos="allTodos"
            @closeModal="isAddTodoModalActive = $event"
        />

        <p class="panel-heading">
            Todo list
        </p>
        <div class="panel-block">
            
            <!-- TODO SEARCH FILTER -->
            <p class="control has-icons-left">
                <input class="input" type="text" @keyup="activeSearchFilter()" @keyup.esc="search = ''" v-model="search" placeholder="Search">
                <span class="icon is-left">
                    <i class="fas fa-search" aria-hidden="true"></i>
                </span>
            </p>

            <button class="button is-info add-todo-btn"
                @click="openAddTodoModal">
                <b-icon icon="plus-circle-outline"></b-icon>
                <span class="add-todo-text">Add todo</span>
            </button>

        </div>

        <!-- TODO CLICK FILTER -->
        <p class="panel-tabs">
            <a @click="activeClickFilter('all')" :class="{ 'is-active': visibleTodos == 'all' }">All</a>
            <a @click="activeClickFilter('completed')" :class="{ 'is-active': visibleTodos == 'completed' }">Completed</a>
            <a @click="activeClickFilter('active')" :class="{ 'is-active': visibleTodos == 'active' }">Active</a>
        </p>
    </div>
</template>

<script>
import AddTodoModal from './modals/AddTodoModal'

export default {
    props: ['visibleTodos', 'allTodos'],
    components: {
        AddTodoModal
    },
    data() {
        return {
            search: '',
            isAddTodoModalActive: false,
            shownTodos: this.visibleTodos
        }
    },
    methods: {
        openAddTodoModal() {
            this.isAddTodoModalActive = true;
        },
        activeSearchFilter() {
            this.$emit('searchFilterWasChanged', this.search);
        },
        activeClickFilter(value) {
            this.shownTodos = value;
            this.$emit('clickFilterWasChanged', this.shownTodos);
        }
    }
}
</script>

<style lang="scss">
.panel {
    max-width: 700px;
    margin: 50px auto;
    .todo-header {    
        .panel-heading {
            text-align: center;
        }
        .add-todo-btn {
            transition: all .5s ease;
            overflow: hidden;
            width: 7%;
            margin-left: 10px;
            &:hover {
                width: 22%;
                box-shadow: 0px 0px 10px 0px rgba(22,125,240,1);
                .icon {
                    margin-left: calc(-0.375em - 1px) !important;
                    margin-right: 0.1875em !important;
                    transform: rotate(-360deg);
                }
                .add-todo-text {
                    width: 100%;
                    opacity: 1;
                }
            }
            .icon {
                transition: all .5s ease;
                margin-left: 0 !important;
                margin-right: 0 !important;
            }
            .add-todo-text {
                transition: all .5s ease;
                width: 0;
                opacity: 0;
            }
        }
    }
}

</style>