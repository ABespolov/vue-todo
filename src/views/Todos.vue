<template>
    <div id="app">
        <router-link to="/">Home</router-link>
        <h4>Todo List</h4>
        <AddTodo
                @add-todo="addTodo"
        />
        <Loader v-if="loading"/>
        <TodoList
                v-else-if="todos.length"
                v-bind:todos="todos"
                @remove-todo="removeTodo"
        />
        <p v-else>No todos!</p>
    </div>
</template>

<script>
    import TodoList from '@/components/TodoList';
    import AddTodo from "@/components/AddTodo";
    import Loader from '@/components/Loader';
    export default {
        name: "app",
        data() {
            return {
                todos: [],
                loading: true,
            }
        },
        mounted() {
            fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
                .then(response => response.json())
                .then(json => {
                    this.todos = json;
                    this.loading = false;
                })
        },
        methods: {
            removeTodo(id) {
                this.todos = this.todos.filter(todo => todo.id !== id);
            },
            addTodo(todo) {
                this.todos.push(todo);
            }
        },
        components: {
            AddTodo,
            TodoList,
            Loader
        }
    };
</script>