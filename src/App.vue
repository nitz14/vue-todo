<template>
  <div id="app">
    <h1 class="ui dividing centered header">Vue.js Todo App</h1>
    <div class='ui three column centered grid'>
      <div class='column'>
        <todo-list v-bind:todos="todos" v-on:create-todo-subtask="createSubtask"></todo-list>
        <create-todo v-on:create-todo="createTodo"></create-todo>
      </div>
    </div>
  </div>
</template>

<script>
import sweetalert from 'sweetalert';
import TodoList from './components/TodoList';
import CreateTodo from './components/CreateTodo';

export default {
  name: 'app',
  components: {
    TodoList,
    CreateTodo,
  },
  data() {
    return {
      todos: [{
        title: 'Todo A',
        project: 'Project A',
        done: false,
        subtask: [],
      }, {
        title: 'Todo B',
        project: 'Project B',
        done: true,
        subtask: [],
      }, {
        title: 'Todo C',
        project: 'Project C',
        done: false,
        subtask: [{ title: 'Sub Todo C 1', done: false }, { title: 'Sub Todo C 2', done: false }],
      }, {
        title: 'Todo D',
        project: 'Project D',
        done: false,
        subtask: [{ title: 'Sub Todo D 1', done: false }],
      }],
    };
  },
  methods: {
    createTodo(newTodo) {
      this.todos.push(newTodo);
      sweetalert('Success!', 'To-Do created!', 'success');
    },
    createSubtask(todo, sub) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].subtask.push(sub);
      sweetalert('Success!', 'Subtask created!', 'success');
    },
  },
};
</script>
