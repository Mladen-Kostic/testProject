
<script>

let id = 0;

export default {
    data() {
        return {
            newTodo: '',
            hideCompleted: false,
            todos: [],
            circleStyles: 'rounded-circle d-inline-block ml-2 mb-1',
            success: true,
            danger: false,
            count: 0
        }
    },
    computed: {
        filteredTodos() {
        return this.hideCompleted
            ? this.todos.filter((t) => !t.done)
            : this.todos
        },
        changeColor() {
            if (this.todos.length >= 5) {
                this.success = false;
                this.danger = true;
            } else if(this.todos.length < 5) {
                this.success = true;
                this.danger = false;
            }
        }
    },
    methods: {
        addTodo() {
            if (this.newTodo) {
                this.todos.push({id: id++, text: this.newTodo, done: false});
            }

            this.changeColor;
            
            this.newTodo = '';
        },
        removeTodo(todo) {
            this.todos = this.todos.filter(t => t !== todo)
            this.changeColor;
        }
    }
}
</script>

<template>

    <form class="secondary" @submit.prevent="addTodo">
        <h3 class="d-inline-block">Todo List</h3>
        <div id="circle" style="width: 10px; height: 10px" :class="[{ 'bg-success': success, 'bg-danger': danger }, circleStyles]"></div>
        <div class="form-group p-2">
            <input id="addTodo" class="form-control col-md-6 d-inline-block mr-3 align-self-center" v-model="newTodo">
            <button style="height: 2.4rem" class="btn btn-success col-md-3 mb-1">Add Todo</button>
        </div>
    </form>
    <ul class="list-group list-group-flush">
        <li v-for="todo in filteredTodos" :key="todo.id" class="list-group-item">
            <div class="row">
                <div class="col-md-1">
                    <input type="checkbox" v-model="todo.done">
                </div>
                <div class="col-md-10">
                    <span :class="{ done: todo.done }">{{ todo.text }}</span>
                </div>

                <div class="col-md-1"><button style="" class="btn btn-danger" @click="removeTodo(todo)">X</button></div>
            </div>
            
        </li>
    </ul>

    <button class="btn btn-secondary mt-2" @click="hideCompleted = !hideCompleted">{{ hideCompleted ? 'Show All' : 'Hide Completed' }}</button>

</template>

<style>
.done {
  text-decoration: line-through;
}
</style>