<template lang="pug">
  div
    input.todo-checkbox(type='checkbox' v-bind:id="'item_' + todoItem.id" v-model="todoItem.done")
    label(v-bind:for="'item_' + todoItem.id")
    span.todo-text {{ todoItem.title }}
    span.delete(@click="deleteItem()")
</template>

<script>
export default {
  name: 'TodoItem',
  props: ['todoItem'],
  methods: {
    deleteItem () {
      this.$emit('todoItemDeleted', this.todoItem);
    }
  }
};
</script>
