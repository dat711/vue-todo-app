<template>
    <main :class="mainClass" :disabled ="showModal">
        <div class="add-todos">
            <h2>Todo Application</h2>
            <form>
                <h3>What you gonna do ??</h3>
                <input class="input-newtodo" type="text" :disabled="showModal" v-model="newTodo" />
                <input type="text" :disabled="showModal" style="display:none" />
                <div></div>
                <button id="button-submit" :disabled="showModal" type="button" @click="addTodo">Submit</button>
            </form>
        </div>
        <div v-if="displayTodos()" class="list-todos">
            <div v-for="todo in todos" class="todo-item" :key="todo.id">
                <input type="checkbox" :disabled="showModal" v-model="todo.done"/>
                <p id="todo-content">{{todo.content}}</p>
                <button id="delete-button" type="button" :disabled="showModal" @click="deleteTodo(todo.id)">Delete</button>
                <button class="edit-button" type="button" :disabled="showModal" @click="showEditTodosModal(todo.id)">edit</button>
            </div>
        </div>
        <div v-else>
            <p>Bro have so much free time</p>
        </div>
    </main>
    <div v-if ="showModal">
            <div class="modal">
                <div class = "modal-header">
                    <h2 id="modal-name">Modal name</h2>
                    <span class="close" @click="showModal=false">&times;</span>
                </div>
                <div class="modal-content">
                    <h3 id = "modal-content-header">What is your new plan??</h3>
                    <input class="input-newtodo" type="text" v-model="newTodo" />
                </div>
                <div class = "modal-footer">
                    <button class="edit-button" @click="editTodos">Update</button>
                </div>
            </div>
        </div>
</template>

<script setup>
    import { ref, onMounted, watch } from 'vue'
    const todos = ref([]);
    const newTodo = ref("");
    const showModal = ref(false);
    const mainClass = ref("");
    const toEditID = ref(0);
    let id = 0;

    const addTodo = () => {
        if (newTodo.value.trim().length < 4 || newTodo.value.trim().length > 40){
            alert("The new todos should have length between 4 and 40 characters")
        }
        id++;
        todos.value.push({
            content: newTodo.value.trim(),
            id:id,
            done:false
        });
        newTodo.value = "";
    }

    const deleteTodo = (id) => {
        todos.value = todos.value.filter((todo) => {return todo.id != id})
    }

    const displayTodos = () => {
        return todos.value.length > 0;
    }
    
    onMounted(()=>{
        todos.value = JSON.parse(localStorage.getItem("todos")) || [];
        id = todos.value.slice(-1).id || 0;
    })
    
    const editTodos = () => {
        debugger
        console.log(toEditID.value);
        var ind = todos.value.findIndex((val) => val.id == toEditID.value);
        var val = todos.value[ind];
        val.content = newTodo.value;
        todos.value.splice(ind,1,val);
        newTodo.value = "";
        showModal.value = false; 
    }

    const showEditTodosModal = (id) => {
        toEditID.value = id;
        showModal.value = true;
    }

</script>

<style scoped>
    .modal-footer {
        display:flex;
        flex-direction: row;
        justify-content: end;
        align-items: center;
        height:40px;
        padding:10px;
    }
    #modal-content-header {
        color:whitesmoke;
    }
    *:disabled {
        opacity:0.4;
    }
    *:disabled > * {
        opacity:0.4;
    }
    #modal-name {
        flex:1;
        color:whitesmoke;
    }
    #delete-button:focus, #edit-button:focus {
        outline-width: 0;
        
    }
    .disabled {
        pointer-events: none;
        opacity: 0.4;
    }
    .modal-header{
        display:flex;
        flex-direction: row;
        align-items: center;
        text-align: left;
        padding: 0px 10px;
        border-bottom: 1px solid #ffffff;
    }
    .modal {
        background-color: #000;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        z-index: 1; /* Sit on top */
        width: 300px; /* Full width */
        height: 180px; /* Full height */
        overflow: auto; /* Enable scroll if needed */
        color:black;
        border-radius:10px;
        border: 1px solid white;
        display:flex;
        flex-direction: column;
    }

    /* Modal Content/Box */
    .modal-content {
        margin: 10px 2.5% ; /* 15% from the top and centered */
        padding: auto;
        width: 95%; /* Could be more or less, depending on screen size */
        flex:1
    }

    /* The Close Button */
    .close {
        color: #8a8888;
        float: right;
        font-size: 28px;
        font-weight: bold;
        
    }

    .close:hover,
    .close:focus {
        color: black;
        text-decoration: none;
        cursor: pointer;
    }
    .edit-button  {
        background-color: blueviolet;
        height:20px;
        display: inline-flex;
        align-items: center;
        text-align: center;
        justify-content: center;
        width: fit-content;
        border-radius: 2px;
        padding:4px;

    }
    .add-todos,.list-todos {
        border:0.1px solid white; 
        border-style: dashed;
        display: flex;
        flex-direction: column;
        padding: 10px;
        border-radius: 10px;
        margin:10px;
    }

    #delete-button {
        background-color: red;
        height:20px;
        display: inline-flex;
        align-items: center;
        text-align: center;
        justify-content: center;
        width: 70px;
        border-radius: 2px;
    }
    .list-todos {
        gap:10px;
    }

    #button-submit {
        background-color: green;
        border-radius: 10px;
        margin: 10px 0px;
        width:100%;
    }

    .todo-item {
        width:100%;
        height:35px;
        padding:10px;
        border-radius:10px;
        border: 1px solid white;
        color:azure;
        display:flex;
        flex-direction: row;
        gap:8px;
        align-items: center;
    }

    #todo-content {
        flex:1;
        text-align: left;
    }

    .input-newtodo {
        width:100%;
        height:35px;
        padding:2px;
        background-color: azure;
        color:black;
        border-radius:10px;
    }

    h2 {
        padding:10px;
    }
</style>
