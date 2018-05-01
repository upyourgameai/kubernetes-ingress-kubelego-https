<template>
  <div style="max-width: 200px; margin: 0 auto;">
    <h4>Todolist</h4>
    <input type="text" v-model="todo_input">
    <button @click="addTodo">Add Todo</button>
    <ul>
      <li style="display: block; text-align: left" v-for="(t, i) in todos">
        <input :checked="t.checked" type="checkbox">
        <span>{{t.title}}</span>
        <button @click="deleteTodo(t)">X</button>
      </li>
    </ul>
  </div>
</template>

<script>
import {HTTP} from '../http/http'
import {_} from 'vue-underscore'
export default {
  name: 'HelloWorld',
  mounted () {
    HTTP.get('todos').then(r => {
      console.log(r.data)
      this.todos = r.data
    }).catch(err => {
      console.log(err)
    })
  },
  data () {
    return {
      todo_input: '',
      todos: []
    }
  },
  methods: {
    addTodo: function () {
      if (this.todo_input == "") return
      var c = this
      var todo = {
        title: this.todo_input,
        checked: false
      }
      HTTP.post('todos', todo).then(r => {
        console.log(r)
        c.todos.push(r.data)
      }).catch(err => {
        console.log(err)
      })
    },
    deleteTodo: function (t) {
      var c = this
      HTTP.delete('todos/' + t.id).then(r => {
        console.log('Todo deleted successfully')
        this.todos = _.reject(c.todos, todo => {return todo.id == t.id} )
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
