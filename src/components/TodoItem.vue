<template>
    <div class="todo-item">
        <div class="todo-item-group">

            <input type="checkbox" v-model="completed" @change="doneEdit">

            <div class="todo-item-label" @dblclick="edit" v-if="!flag" :class="{ completed : completed }">
                {{title}}
            </div>

            <input v-else type="text" v-model="title" class="todo-item-input" @blur="doneEdit" @keyup.enter="doneEdit" @keyup.esc="exitEdit">
        </div>

        <div class="remove-item" @click="removeTodo(index)">
            &times;
        </div>
    </div>
</template>

<script>
export default {
    name: 'todo-item',

    props:{
        todo:{
            type: Object,
            required: true,
        },
        index:{
            type: Number,
            required: true,
        },
        checkAll:{
            type: Boolean,
            required: true
        }
    },

    data(){
        return {
            'id': this.todo.id,
            'title': this.todo.title,
            'completed': this.todo.completed,
            'flag': this.todo.flag,
            'cacheTitle': ''
        }
    },

    directives: {
        focus: {
        inserted: function (el) {
                el.focus()
            }
        }
    },

    watch:{
        checkAll(){
            this.completed = this.checkAll ? true : this.todo.completed
        }
    },

    methods:{
        removeTodo(index){
            this.$emit('removeTodo',index)
        },

        edit(){
            this.cacheTitle = this.title,
            this.flag = true
        },

        doneEdit(){
            if(this.title.trim() == ''){
                this.title = this.cacheTitle
            }

            this.flag = false

            this.$emit('finishedEdit',{
                'index': this.index,
                'todo':{
                    'id': this.id,
                    'title': this.title,
                    'completed': this.completed,
                    'flag': this.flag,
                }
            })
        },

        exitEdit(){
            this.title = this.cacheTitle,
            todo.flag = false
        },
    }
}
</script>