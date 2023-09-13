<script setup>
    import {ref, computed} from 'vue'

    let id = 0

    const newTodo = ref('')
    const hideCompleted = ref(false)
    const todos = ref([
        {id: id++, text: `Add something to the list`, done: false}
    ])
    const filterTodos = computed(() => {
        return hideCompleted.value ? todos.value : todos.value.filter((t) => !t.done)
    })

    function addTodo(){
        todos.value.push({id: id++, text: newTodo.value, done: false})
        newTodo.value = ''
    }

    function removeTodo(todo){
        todos.value = todos.value.filter((t) => t !== todo)
    }
</script>

<template>
    <div class="container">
        <form @submit.prevent="addTodo">
            <input v-model="newTodo" required>
            <button>Add Todo</button>    
        </form>
        <ul>
            <li v-for="todo in filterTodos" :key="todo.id">
                <input type="checkbox"  v-model="todo.done">
                <span :class="{ done: todo.done }">{{ todo.text }}</span>
                <button @click="removeTodo(todo)">X</button>
            </li>
        </ul>
        <button v-on:click="hideCompleted = !hideCompleted">{{ hideCompleted ? `hide completed items` : `show all` }}</button>
    </div>
</template>

<style>
    .container{
        display: flex;
        /* justify-content: center; */
        flex-direction: column;
        align-items: center;
        border: 2px solid #fff;
        padding: 50px;
        border-radius: 20px;
        height: 80vh;
        width: 90vw;
        background: #000000aa;
        backdrop-filter: blur(5px);
    }

    form{
        width: 100%;
        /* border: 3px solid #fff; */
    }

    input{
        background: inherit;
        border: none;
        border-bottom: 2px solid #fff;
        height: 50px;
        width: 70%;
        /* border-bottom-left-radius: 20px; */
        /* border-top-left-radius: 20px; */
        color: #ddd;
        padding: 0;
    }
    
    button{
        height: 50px;
        background: #0a0;
        color: #fff;
        border: none;
        /* border-top-right-radius: 20px; */
        /* border-bottom-right-radius: 20px; */
        width: 20%;
    }

    ul{
        list-style: none;
    }

    li{
        margin-top: 30px;
        color: #ddd;
        border: 2px solid #fff;
        padding: 10px 20px;
        border-radius: 20px;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 90%;
        width: max-content;
        display: grid;
        grid-template-columns: 1fr 8fr 1fr;
    }

    li button{
        height: 30px;
        width: 30px;
        border-radius: 50%;
        margin-left: 5px;
        background: #a00;
    }

    .done
    {
        text-decoration: line-through;
    }
</style>