<template>
  <div class='ui centered card'>
    <div class="content" v-show="!isEditing">
      <div class='header'>
          {{ todo.title }}
      </div>
      <div class='meta'>
          {{ todo.project }}
      </div>
      <div class='meta' v-if="todo.subtask.length > 0">
          Pending SubTasks: {{todo.subtask.filter(subtask => {return subtask.done === false}).length}}
      </div>
      <div class='meta' v-if="todo.subtask.length > 0">
          Completed Tasks: {{todo.subtask.filter(subtask => {return subtask.done === true}).length}}
      </div>
      <div class='extra content'>
          <span class='right floated edit icon' v-on:click="showForm">
          <i class='edit icon'></i>
        </span>
        <span class='right floated trash icon' v-on:click="deleteTodo(todo)">
          <i class='trash icon'></i>
        </span>
        <span class='right floated plus icon' v-on:click="createSubtask(todo)">
          <i class='plus icon'></i>
        </span>
      </div>
    </div>
    <div class="content" v-show="isEditing">
      <div class='ui form'>
        <div class='field'>
          <label>Title</label>
          <input type='text' v-model="todo.title" >
        </div>
        <div class='field'>
          <label>Project</label>
          <input type='text' v-model="todo.project" >
        </div>
        <div class='ui two button attached buttons'>
          <button class='ui basic blue button' v-on:click="hideForm">
            Close X
          </button>
        </div>
      </div>
    </div>
    <div class='ui bottom attached green basic button' v-show="!isEditing && todo.done" v-on:click="pendingTodo(todo)" disabled>
        Completed
    </div>
    <div class='ui bottom attached red basic button' v-on:click="completeTodo(todo)" v-show="!isEditing && !todo.done">
        Pending
    </div>

    <!-- subtask list -->
    <div class='ui centered header' v-if="todo.subtask.length > 0">
      SUBTASK
    </div>
    <div v-for="sub in todo.subtask" class='ui centered card' v-if="todo.subtask.length > 0">
      <div class='ui centered card'>
        <div class='header'>
          <div class="content">
            <div class='header'>
              {{ sub.title }}
            </div>
            <div class='extra content'>
              <span class='right floated trash icon' v-show="!isEditing" v-on:click="deleteSub(todo, sub)">
                <i class='trash icon'></i>
              </span>
            </div>
          </div>
        </div>
        <div class='ui bottom attached green basic button' v-show="!isEditing && sub.done" v-on:click="pendingSub(todo, sub)" disabled>
          Completed
        </div>
        <div class='ui bottom attached grey basic button' v-show="isEditing && sub.done" disabled>
          Completed
        </div>
        <div class='ui bottom attached red basic button' v-on:click="completeSub(todo, sub)" v-show="!isEditing && !sub.done">
          Pending
        </div>
        <div class='ui bottom attached grey basic button' v-show="isEditing && !sub.done">
          Pending
        </div>
      </div>
    </div>
    <!-- end of subtakslist -->

     <!-- create subtask element -->
     <div class='ui basic content right aligned segment'>
      <div class='ui right card' v-show="isCreatingSub">
        <div class='content'>
          <div class='ui form'>
            <div class='field'>
              <label>Subtask Title</label>
              <input v-model="subtasktitleText" type='text'>
            </div>
            <div class='ui two button attached buttons'>
              <button class='ui basic blue button' v-on:click="sendSubtaskForm(todo)">
                Create
              </button>
              <button class='ui basic red button' v-on:click="closeSubtaskForm">
                Cancel
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- end of subtask element -->
  </div>
</template>

<script type="text/javascript">
  export default {
    props: ['todo'],
    data() {
      return {
        isEditing: false,
        isCreatingSub: false,
        subtasktitleText: '',
      };
    },
    methods: {
      pendingTodo(todo) {
        this.$emit('pending-todo', todo);
      },
      completeTodo(todo) {
        this.$emit('complete-todo', todo);
      },
      deleteTodo(todo) {
        this.$emit('delete-todo', todo);
      },
      showForm() {
        this.isEditing = true;
      },
      hideForm() {
        this.isEditing = false;
      },
      createSubtask() {
        this.isCreatingSub = true;
      },
      closeSubtaskForm() {
        this.isCreatingSub = false;
      },
      sendSubtaskForm(todo) {
        if (this.subtasktitleText.length > 0) {
          const title = this.subtasktitleText;
          this.$emit('create-todo-subtask', todo, {
            title,
            done: false,
          });
          this.subtasktitleText = '';
          this.isCreatingSub = false;
          this.task = '';
        }
      },
      deleteSub(todo, subtask) {
        this.$emit('delete-sub', todo, subtask);
      },
      pendingSub(todo, subtask) {
        this.$emit('pending-sub', todo, subtask);
      },
      completeSub(todo, subtask) {
        this.$emit('complete-sub', todo, subtask);
      },
    },
  };
</script>
