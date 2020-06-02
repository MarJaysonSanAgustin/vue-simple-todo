<template>
  <section class="todo-wrapper">

      <h2 class="todo-title">League Game Checklist <br> <small>{{ today.day }} - {{ today.date }}</small></h2>

      <form @keydown.enter.prevent="">
        <input type="text" class="input-todo" v-bind:class="{ active: newTodo }" placeholder="Push Mid" v-model="newTodo" v-on:keyup.enter="addItem">
        <div class="btn btn-add" v-bind:class="{ active: newTodo }"  @click="addItem">+</div>
      </form>

      <div v-if="pending.length > 0">
        <p class="status busy">You have {{ pending.length }} pending item<span v-if="pending.length>1">s</span></p>

        <transition-group name="todo-item" tag="ul" class="todo-list">
          <li v-for="(item) in pending" v-bind:key="item.title">
            <input class="todo-checkbox" v-bind:id="'item_' + item.id" v-model="item.done" type="checkbox">
            <label v-bind:for="'item_' + item.id"></label>
            <span class="todo-text">{{ item.title }}</span>
            <span class="delete" @click="deleteItem(item)"></span>
          </li>
        </transition-group>

      </div>

      <transition name="slide-fade">
        <p class="status free" v-if="!pending.length" ><img src="../assets/ez.png" alt="celebration">Chillin' Like a Villain.</p>
      </transition>

      <div v-if="completed.length > 0 && showComplete">

        <p class="status">Completed tasks: {{ completedPercentage }}</p>

        <transition-group name="todo-item" tag="ul" class="todo-list archived">
          <li v-for="(item) in completed" v-bind:key="item.title">
            <input class="todo-checkbox" v-bind:id="'item_' + item.id" v-model="item.done" type="checkbox">
            <label v-bind:for="'item_' + item.id"></label>
            <span class="todo-text">{{ item.title }}</span>
            <span class="delete" @click="deleteItem(item)"></span>
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
export default {
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
      handler: function (updatedList) {
        localStorage.setItem('todos', JSON.stringify(updatedList));
      },
      deep: true
    }
  },
  computed: {
    pending: function () {
      return this.todoList.filter(function (item) {
        return !item.done;
      });
    },
    completed: function () {
      return this.todoList.filter(function (item) {
        return item.done;
      });
    },
    completedPercentage: function () {
      return (Math.floor((this.completed.length / this.todoList.length) * 100)) + '%';
    },
    today: function () {
      const weekday = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      let today = new Date();
      let dd = today.getDate();
      let mm = today.getMonth() + 1;
      const yyyy = today.getFullYear();

      if (dd < 10) {
        dd = '0' + dd;
      }

      if (mm < 10) {
        mm = '0' + mm;
      }

      today = {
        day: weekday[today.getDay()],
        date: mm + '-' + dd + '-' + yyyy
      };

      return (today);
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
