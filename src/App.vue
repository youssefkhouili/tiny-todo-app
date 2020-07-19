<template>
  <div id="app" class="container">
    <h2 class="text-center my-4">{{ create_msg }}</h2>
    <form @submit.prevent="addToDo">
      <div class="form-group">
        <label for="todo">Todo</label>
        <input v-model="toDo" type="text" class="form-control" id="todo" aria-describedby="emailHelp" placeholder="Enter What Do You Want To Do">
      </div>
      <button type="submit" class="btn btn-primary">Add To Do</button>
    </form>
    <ul class="list-group mt-4">
      <li :key="todo.title" v-for="(todo, index) in todos" class="list-group-item">
        <span :class="{'line-through':todo.done}">{{ todo.title }}</span>
        <button v-if="! todo.done" class="btn btn-success float-right" @click="doneToDo(todo)">Done</button>
        <button v-if="todo.done" @click="deleteToDo(index)" class="btn btn-danger float-right">Delete To Do</button>
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: "App",
  components: {
    
  },
  data() {
    return {
      create_msg: 'To Do App',
      toDo: '',
      todos: []
    }
  },
  watch: {
    todos: {
      handler() {
        localStorage.todos = JSON.stringify(this.todos)
      },
      deep: true
    }
  },
  mounted() {
    if (localStorage.todos) {
      this.todos = JSON.parse(localStorage.todos)
    }
  },
  methods: {
    addToDo() {
      this.todos.push({
        title: this.toDo,
        done: false
      }); 
      this.toDo = '';
    },
    doneToDo(todo) {
      todo.done = true
    },
    deleteToDo(index) {
      setTimeout(() => {
        this.todos.splice(index, 1)
      }, 1000)
    }
  }
};
</script>

<style lang="scss">
  .line-through {
    text-decoration: line-through;
  }
</style>
