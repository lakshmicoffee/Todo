<template>
    <h1>ToDo List</h1>
    <form>
        <input
            v-model="TodoItems"
            name="TodoItems"
            autocomplete="off"
            placeholder="Enter To Do Items"
        >
        <button @click="addTodo(todo)" class="add">Add</button>
    </form>
    <h2>List Items</h2>
    <div class="container">
    <ul>
        <li
            v-for="(todo, i) in todos"
            :key="i">
            <span
                :class="{ done: todo.done }"
            >{{ todo.content }}</span>
            <button @click="removeTodo(i)" class="delete">Delete</button>
        </li>
    </ul>
    <h4 v-if="todos.length === 0">There is no content........</h4>
    </div>
</template>

<script>
    import { ref } from 'vue';
    export default {
        name: 'App',
        setup () {
            const TodoItems = ref('');
            const defaultItem = [{
                content: 'This is first Todo item'
            }]
            const todosData = JSON.parse(localStorage.getItem('todos')) || defaultItem;
            const todos = ref(todosData);
            function addTodo () {
                if (TodoItems.value) {
                    todos.value.push({
                        done: false,
                        content: TodoItems.value
                    });
                    TodoItems.value = '';
                }
                saveData();
            }
            function doneTodo (todo) {
                todo.done = !todo.done
                saveData();
            }
            function removeTodo (index) {
                todos.value.splice(index, 1);
                saveData();
            }
            function saveData () {
                const storageData = JSON.stringify(todos.value);
                localStorage.setItem('todos', storageData);
            }
            return {
                todos,
                TodoItems,
                addTodo,
                doneTodo,
                removeTodo,
                saveData
            }
        }
    }
</script>