<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
// @ is an alias to /src
import Todos from '@/components/Todos.vue';
import AddTodo from '@/components/AddTodo.vue';
import axios from 'axios';

export default {
  name: 'home',
  components: {
    Todos,
    AddTodo,

  },
  data(){
    return {
      todos: [],
      // result: []
    }
  },
  methods:{
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      // eslint-disable-next-line
        .then( this.todos = this.todos.filter(todo => todo.id !== id))
        .catch(err => console.log(err));
        
    },
    //  deleteTodo: function(result, id) {
    //   axios.delete('https://my-json-server.typicode.com/json/posts/' + id)
    //   .then(response => {
    //     this.result.splice(id, 1)
    //     console.log(this.result);
    //   });
    // },

    // getResult: function() {
    //   // this.results = 'Loading...';
    //   axios.get('https://my-json-server.typicode.com/json/db')
    //   .then(response => {
    //     // console.log(response.data);
    //     this.result = response.data.posts;
    //     console.log(this.result);
    //   });
    // },

    addTodo(newTodo) {
      const{ title, completed } = newTodo;
      axios.post('https://jsonplaceholder.typicode.com/todos',{
        title,
        completed
      })
      .then(res => this.todos = [...this.todos, res.data])
      .catch(err => console.log(err));
    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
    .then(res => this.todos = res.data)
    .catch(err => console.log(err));
  }
}
</script>


<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }
  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }
</style>
