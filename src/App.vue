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
                        <a href="https://github.com/martinvlcek/todoapp-vue" class="github-link" target="_blank">
                            <i class="fab fa-github"></i>
                        </a>
                    </h2>
                </div>
            </div>
        </section>

        <nav class="panel">

            <todo-header
                :visibleTodos="visibleTodos"
                :allTodos="allTodos"
                @searchFilterWasChanged="search = $event"
                @clickFilterWasChanged="visibleTodos = $event"
            />

            <todo-body
                :visibleTodosNext="visibleTodos"
                :search="search"
                @getAllTodos="allTodos = $event"
            />

        </nav>

    </div>
</template>

<script>
import TodoHeader from './components/TodoHeader'
import TodoBody from './components/TodoBody'

export default {
    data() {
        return {
            allTodos: [],
            visibleTodos: 'all',
            search: ''
        }
    },
    components: {
        TodoHeader,
        TodoBody
    }
}
</script>

<style lang="scss">
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    .hero {
        text-align: center;
        .github-link {
            &:hover {
                .fa-github {
                    transform: scale(1.3);
                }
            }
            .fa-github {
                 transition: all .2s ease-in-out;
            }
        }
    }
}

</style>
