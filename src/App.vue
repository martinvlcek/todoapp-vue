<template>
    <div id="app">
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

                <button class="button is-info add-todo-btn">
                    <b-icon icon="plus-circle-outline"></b-icon>
                    <span>Add todo</span>
                </button>

            </div>
            <p class="panel-tabs">
                <a class="is-active">All</a>
                <a>Completed</a>
                <a>Uncompleted</a>
            </p>
            <a class="panel-block">
                <div class="left-block">
                    <div class="field">
                        <b-checkbox :value="true" v-model="isCompleted"
                        type="is-success">
                        </b-checkbox>
                    </div>
                    <p class="todo-text" :class="{ 'todo-done' : isCompleted }">
                        bulma
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
            isCompleted: true
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
