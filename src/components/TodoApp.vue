<template>
    <div>
        <p style="text-align:center">{{msg}}</p>

        <input type="text" class="inputs" placeholder="whats to be done" v-model="newTodo" @keyup.enter="addTodo">

        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">

            <todo-item v-for="(todo,index) in todosFiltered" :key="todo.id" :index="index" :todo="todo" :checkAll="!anyRemaining" @removeTodo="removeTodo" @finishedEdit="finishedEdit">
            </todo-item>

        </transition-group>

        <div class="extra-container">
            <div>
                <label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check All</label>
            </div>
            <div>{{ remaining }} items left</div>
        </div>

        <div class="extra-container">
        <div>
            <button :class="{ active: anchor == 'all' }" @click="anchor = 'all'">All</button>
            <button :class="{ active: anchor == 'active' }" @click="anchor = 'active'">Active</button>
            <button :class="{ active: anchor == 'completed' }" @click="anchor = 'completed'">Completed</button>
        </div>

        <div>
            <!-- <transition name="fade"> -->
            <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>

            <!-- </transition> -->
        </div>

        </div>
    </div>
</template>

<script>
import TodoItem from './TodoItem'

export default {
    name : "todo-app",
    components:{
        TodoItem
    },
    data(){
        return {
            msg: "Todos page Here!",
            newTodo : '',
            newIdForTodo : 3,
            cacheTitle : '',
            anchor: 'all',
            todos : [
                {
                    'id' : 1,
                    'title' : "Finish Uwu section 1",
                    'completed' : false,
                    'flag' : false
                },
                {
                    'id' : 2,
                    'title' : "Finish Uwu section 2",
                    'completed' : true,
                    'flag' : false
                },
            ]
        }
    },

    computed:{
        remaining(){
            return this.todos.filter(todo => !todo.completed).length
        },

        anyRemaining(){
            return this.remaining != 0
        },

        todosFiltered(){
            if(this.anchor == 'all'){
                return this.todos
            }else if(this.anchor == 'active'){
                return this.todos.filter(todo => !todo.completed)
            }else if(this.anchor == 'completed'){
                return this.todos.filter(todo => todo.completed)
            }
        },

        showClearCompletedButton() {
            return this.todos.filter(todo => todo.completed).length > 0
        }
    },

    methods:{
        addTodo(){
            //validation
            if(this.newTodo.trim() == 0){
                return
            }

            this.todos.push({
                id : this.newIdForTodo,
                title : this.newTodo,
                completed : false,
                flag : false
            })

            this.newTodo = ''
            this.newIdForTodo++
        },

        removeTodo(index){
            this.todos.splice(index,1);
        },

        finishedEdit(data){
            this.todos.splice(data.index, 1, data.todo)
        },

        checkAllTodos(){
            this.todos.forEach((todo) => todo.completed = event.target.checked);

        },

        clearCompleted() {
            this.todos = this.todos.filter(todo => !todo.completed)
        }
    }
}
</script>

<style lang="scss">

    @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

    .inputs {
        width: 100%;
        padding: 8px 8px;
        font-size: 18px;
        margin-bottom: 16px;
        margin-top: 16px;
        &:focus {
        outline: 0;
        }
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 0.6s;
    }

    .not-complete{
        color: orange
    }

    .remove-item {
        cursor: pointer;
        margin-left: 14px;

        &:hover {
        color: red;
        }
    }

    .todo-item-group {
        display: flex;
        align-items: center;
    }

    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }
    .todo-item-input {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc; //override defaults
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        &:focus {
        outline: none;
        }
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
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }
    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
        &:hover {
        background: lightgreen;
        }
        &:focus {
        outline: none;
        }
    }
    .active {
        background: lightgreen;
    }
    // CSS Transitions
    .fade-enter-active, .fade-leave-active {
        transition: opacity .2s;
    }
    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>