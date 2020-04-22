<template>
    <div>
        
        <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newToDo" @keyup.enter="addTodo">

        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">

            <to-do-item v-for="(todo, index) in todoListFiltered" 
                        :key="todo.id"
                        :todo="todo"
                        :index="index"
                        :checkAll="!uncompletedTodos.length" 
                        @removedTodo="removeTodo"
                        @finishedEdit="finishedEdit"/>

        </transition-group>    

        <hr>

        <div class="extra-container">
            <div>
                <label><input type="checkbox" :checked="!uncompletedTodos.length" @change="checkAllTodos"> Check All </label>
            </div>
            <div> {{ uncompletedTodos.length }} items left </div>
        </div>

        <div class="extra-container">
            <div>    
                <button :class="{ active: filter == 'all' }" @click="filter = 'all'"> All </button>
                <button :class="{ active: filter == 'active' }" @click="filter = 'active'"> Active </button>
                <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'"> Completed </button>
            </div>

            <div>
                <transition name="fade">
                    <button v-if="showClearComletedButton" @click="clearCompleted"> Clear Completed </button>
                </transition>   
            </div>
        </div>

    </div>    
</template>

<script>
import ToDoItem from './ToDoItem';

export default {
    name: 'ToDoList',

    components: {
        ToDoItem
    },

    data() {
        return {
            newToDo: '',
            beforeEditCache: '',
            idForTodo: '3',
            filter: '',
            todoList: [
                {
                    'id': 1,
                    'title': 'Finish Vue Screencast',
                    'completed': false,
                    'editing': false
                },
                {
                    'id': 2,
                    'title': 'Take over world',
                    'completed': false,
                    'editing': false
                },
            ]
        }
    },

    computed: {
        uncompletedTodos() {
            return this.todoList.filter(todo => !todo.completed);
        },

        todoListFiltered() {
            if (this.filter == 'all') {
                return this.todoList;
                
            } else if (this.filter == 'active') {
                return this.todoList.filter(todo => !todo.completed)
            
            } else if (this.filter == 'completed'){
                return this.todoList.filter(todo => todo.completed)
            }

            return this.todoList;
        },

        showClearComletedButton() {
            return this.todoList.filter(todo => todo.completed).length > 0;
        },
        

         
        
    },

    methods: {
        addTodo() {
            if (this.newToDo.trim().length == 0) {
                return;
            }

            this.todoList.push({
                id: this.idForTodo,
                title: this.newToDo,
                completed: false,                
            })

            this.newToDo = '';
            this.idForTodo++;
        },

        removeTodo(index) {
            this.todoList.splice(index, 1);
        },        

        checkAllTodos() {
            this.todoList.forEach(todo => todo.completed = event.target.checked)
        },

        clearCompleted() {
            this.todoList = this.todoList.filter(todo => !todo.completed);
        },

        finishedEdit(data) {
            this.todoList.splice(data.index, 1, data.todo)
        }
    }
}
</script>

<style lang="scss">

    @import url('https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css');

    .todo-input {
        width: 100%;
        padding: 10px 18 px;
        font-size: 18px;
        margin-bottom: 16px;        
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: .3s;
    }

    .remove-item {
        cursor: pointer;
        margin-left: 14px;
        &:hover {
            color: black;
        }
    }

    .todo-item-left {
        display: flex;
        align-items: center;
    }

    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }

    .todo-item-edit {
        font-size: 24px;
        color: 2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Arial, Helvetica, sans-serif;
    }

    .completed {
        text-decoration: line-through;
        color: grey;
    }

    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
    }

    .active {
        background-color: rgb(0, 255, 64);
    }

    //CSS transitions
    .fade-enter-active, .fade-leave-active {
        transition: opacity .2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
    
</style>