<template>
  <div>
    <p class="tasks">Completed Tasks: {{todos.filter(todo => {return todo.done === true}).length}}</p>
    <p class="tasks">Pending Tasks: {{todos.filter(todo => {return todo.done === false}).length}}</p>
    <todo v-on:create-todo-subtask="createSubtask" v-on:delete-todo="deleteTodo" v-on:pending-todo="pandingTodo" v-on:complete-todo="completeTodo" v-on:delete-sub="deleteSub" v-on:pending-sub="pendingSub" v-on:complete-sub="completeSub" v-for="todo in todos" :todo.sync="todo">
    </todo>
  </div>
</template>

<script type = "text/javascript" >
import sweetalert from 'sweetalert';
import Todo from './Todo';

export default {
  props: ['todos'],
  components: {
    Todo,
  },
  methods: {
    deleteTodo(todo) {
      sweetalert({
        title: 'Are you sure?',
        text: 'This To-Do will be permanently deleted!',
        type: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#DD6B55',
        confirmButtonText: 'Yes, delete it!',
        closeOnConfirm: false,
      },
      () => {
        const todoIndex = this.todos.indexOf(todo);
        this.todos.splice(todoIndex, 1);
        sweetalert('Deleted!', 'Your To-Do has been deleted.', 'success');
      });
    },
    completeTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = true;
      const subtask = this.todos[todoIndex].subtask;
      if (subtask.length > 0) {
        for (let i = subtask.length - 1; i >= 0; i -= 1) {
          subtask[i].done = true;
        }
      }
      sweetalert('Success!', 'To-Do completed!', 'success');
    },
    pandingTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = false;
      sweetalert('Success!', 'To-Do move to pannding!', 'success');
    },
    createSubtask(todo, subtask) {
      this.$emit('create-todo-subtask', todo, subtask);
    },
    deleteSub(todo, sub) {
      sweetalert({
        title: 'Are you sure?',
        text: 'This To-Do will be permanently deleted!',
        type: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#DD6B55',
        confirmButtonText: 'Yes, delete it!',
        closeOnConfirm: false,
      },
      () => {
        const todoIndex = this.todos.indexOf(todo);
        const subIndex = this.todos[todoIndex].subtask.indexOf(sub);
        this.todos[todoIndex].subtask.splice(subIndex, 1);
        sweetalert('Deleted!', 'Your Sub-Task has been deleted.', 'success');
      });
    },
    pendingSub(todo, sub) {
      const todoIndex = this.todos.indexOf(todo);
      const subIndex = this.todos[todoIndex].subtask.indexOf(sub);
      this.todos[todoIndex].subtask[subIndex].done = false;
      sweetalert('Success!', 'Sub-Task move to pannding!', 'success');
    },
    completeSub(todo, sub) {
      const todoIndex = this.todos.indexOf(todo);
      const subIndex = this.todos[todoIndex].subtask.indexOf(sub);
      this.todos[todoIndex].subtask[subIndex].done = true;
      sweetalert('Success!', 'Sub-Task completed!', 'success');
    },
  },
};
</script>

<style scoped>
p.tasks {
  text-align: center;
}
</style>

