<script>
//import ChildComp from './ChildComp.vue'

let id = 0

export default {

  //components: {
  //  ChildComp
  //},

  data() {
    return {
      message: 'Hello world !',
      count: 0,
      titleClass : 'title',
      text:'',
      awesome:true,

      newTodo:'',
      hideCompleted:false,
      todos:[
        { id: id++, text: 'Learn HTML', done: true },
        { id: id++, text: 'Learn JavaScript', done: true },
        { id: id++, text: 'Learn Vue', done: false }
      ],

      todoId: 1,
      todoData: null,

    //Import Child
    //greeting : 'Hello from parent',

    //childMsg: 'No child msg yet',
    //msg: 'from parent',
    }
  },

  computed: {
    filteredTodos() {
      return this.hideCompleted
        ? this.todos.filter((t) => !t.done)
        : this.todos
    }
  },

  methods: {

    increment() {
    this.count++
    },
  
    toggle() {
      this.awesome = !this.awesome
    },

    addTodo() {
      this.todos.push({ id: id++, text: this.newTodo, done: false })
      this.newTodo = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    },

    async fetchData() {
      this.todoData = null
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${this.todoId}`
      )
      this.todoData = await res.json()
    }
  },

  mounted() {
    this.$refs.p.textContent = 'mounted!',

    this.fetchData()
  },


  watch: {
    todoId() {
      this.fetchData()
    }
  }

}
</script>


<template>
  <!--TITRE-->
  <h1 :class="titleClass">{{ message}}</h1>

  <!--BUTTON-->
  <button v-on:click="increment">count is : {{ count }}</button>

  <!--INPUT-->
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>

  <!--BUTTON-->
  <button @click="toggle">toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>

  <!--FORM-->
   <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>

  <!--PARA-->
  <p ref="p">hello</p>

  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>

  <!--Import Child Component-->
  <!--<ChildComp : msg="greeting" />-->
  <!--<ChildComp /> <p>{{ childMsg }}</p> -->
  <!--<ChildComp>Message: {{ msg }}</ChildComp>-->

</template>


<style>
.title {
  color: red;
}
.done {
  text-decoration: line-through;
}
</style>