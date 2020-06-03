<template>
  <section class="todo-wrapper">

    <TodoHeader title="League Sure Win Tasks" />

    <form @keydown.enter.prevent="">
      <input
        type="text"
        class="input-todo"
        placeholder="Push Mid"
        v-model="newTodo"
        v-on:keyup.enter="addItem"
        v-bind:class="{ active: newTodo }"
      >
      <div class="btn btn-add" v-bind:class="{ active: newTodo }"  @click="addItem">+</div>
    </form>

    <div v-if="pending.length > 0">
      <p class="status busy">You have {{ pending.length }} pending item<span v-if="pending.length>1">s</span></p>

      <transition-group name="todo-item" tag="ul" class="todo-list">
        <li v-for="item in pending" v-bind:key="item.title">
          <TodoItem :todoItem="item" @todoItemDeleted="deleteItem"/>
        </li>
      </transition-group>

    </div>

    <transition name="slide-fade">
      <p class="status free" v-if="!pending.length" ><img src="../assets/ez.png" alt="celebration">Chillin' Like a Villain.</p>
    </transition>

    <div v-if="completed.length > 0 && showComplete">

      <p class="status">Completed tasks: {{ completedPercentage }}</p>

      <transition-group name="todo-item" tag="ul" class="todo-list archived">
        <li v-for="item in completed" v-bind:key="item.title">
          <TodoItem :todoItem="item" @todoItemDeleted="deleteItem"/>
        </li>
      </transition-group>

    </div>

    <div class="control-buttons">

      <div class="btn btn-secondary" v-if="completed.length > 0" @click="toggleShowComplete"><span v-if="!showComplete">Show</span><span v-else>Hide</span> Complete</div>

      <div class="btn btn-secondary" v-if="todoList.length > 0" @click="clearAll">Clear All</div>

    </div>

  </section>

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
      todoList: [
        { id: 0, title: 'Leash', done: false },
        { id: 1, title: 'Farm Monsters', done: false },
        { id: 2, title: 'Gank Top', done: false },
        { id: 3, title: 'Funnel', done: false },
        { id: 4, title: 'Kill Rift Herald', done: false },
        { id: 5, title: 'Kill Mountain Drake', done: false },
        { id: 6, title: 'Achieve 100 CS', done: false },
        { id: 7, title: 'GG', done: false }
      ],
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
