<template>
    <div>
        <input class="todo-checkbox" v-bind:id="'item_' + todoItem.id" v-model="todoItem.done" type="checkbox">
        <label v-bind:for="'item_' + todoItem.id"></label>
        <span class="todo-text">{{ todoItem.title }}</span>
        <span class="delete" @click="deleteItem()"></span>
    </div>
</template>

<script>
export default {
  name: 'TodoItem',
  props: ['todoItem'],
  methods: {
    deleteItem () {
      console.log(this.todoItem);
      this.$emit('todoItemDeleted', this.todoItem);
    }
  }
};
</script>
