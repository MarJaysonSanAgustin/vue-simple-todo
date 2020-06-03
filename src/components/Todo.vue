<template lang="pug">
  section.todo-wrapper

    TodoHeader(title="League Sure Win Tasks")

    form(@keydown.enter.prevent="")
      input.input-todo(type="text" placeholder="Push Mid" v-model="newTodo" v-on:keyup.enter="addItem" v-bind:class="{ active: newTodo }")
      div.btn.btn-add(v-bind:class="{ active: newTodo }"  @click="addItem") +

    div(v-if="pending.length > 0")
      p.status.busy
        span You have {{ pending.length }} pending item
        span(v-if="pending.length>1") s

      transition-group.todo-list(name="todo-item" tag="ul")
        li(v-for="item in pending" v-bind:key="item.title")
          TodoItem(:todoItem="item" @todoItemDeleted="deleteItem")

    transition(name="slide-fade")
      p.status.free(v-if="!pending.length")
        img(src="../assets/ez.png" alt="celebration")
        span Chillin' Like a Villain.

    div(v-if="completed.length > 0 && showComplete")
      p.status Completed tasks: {{ completedPercentage }}

      transition-group.todo-list.archived(name="todo-item" tag="ul")
        li(v-for="item in completed" v-bind:key="item.title")
          TodoItem(:todoItem="item" @todoItemDeleted="deleteItem")

    div.control-buttons
      div.btn.btn-secondary(v-if="completed.length > 0" @click="toggleShowComplete")
        span(v-if="!showComplete") Show
        span(v-else) Hide
        span Complete

      div.btn.btn-secondary(v-if="todoList.length > 0" @click="clearAll") Clear All
</template>

<script>
import TodoHeader from './TodoHeader.vue';
import TodoItem from './TodoItem.vue';

export default {
  name: 'Todo',
  components: {
    TodoHeader,
    TodoItem
  },
  data () {
    return {
      todoList: [],
      newTodo: '',
      showComplete: false
    };
  },
  mounted () {
    this.getTodos();
  },
  watch: {
    todoList: {
      handler (updatedList) {
        localStorage.setItem('todos', JSON.stringify(updatedList));
      },
      deep: true
    }
  },
  computed: {
    pending () {
      return this.todoList.filter(item => !item.done);
    },
    completed () {
      return this.todoList.filter(item => item.done);
    },
    completedPercentage () {
      return `${Math.floor((this.completed.length / this.todoList.length) * 100)}%`;
    }
  },
  methods: {
    getTodos () {
      if (localStorage.getItem('todos')) {
        this.todoList = JSON.parse(localStorage.getItem('todos'));
      }
    },
    addItem () {
      if (this.newTodo) {
        this.todoList.unshift({
          id: this.todoList.length,
          title: this.newTodo,
          done: false
        });
      }
      this.newTodo = '';
      return true;
    },
    deleteItem (item) {
      this.todoList.splice(this.todoList.indexOf(item), 1);
    },
    toggleShowComplete () {
      this.showComplete = !this.showComplete;
    },
    clearAll () {
      this.todoList = [];
    }
  }
};
</script>
