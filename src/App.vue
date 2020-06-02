<template>
  <div id="app">
    <Todo />
  </div>
</template>

<script>
import Todo from './components/Todo.vue';

export default {
  name: 'App',
  components: {
    Todo
  }
};
</script>

<style lang="scss" src="./App.scss">
</style>
