<template lang="pug">
  div
    h2.todo-title {{ title }}
      br
      small {{ today.day }} - {{ today.date }}
</template>

<script>
export default {
  name: 'TodoHeader',
  props: {
    title: String
  },
  computed: {
    today () {
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
        date: dd + '/' + mm + '/' + yyyy
      };

      return (today);
    }
  }
};
</script>

<style lang="scss" scoped>
    .todo-title{
        font-size: 2em;
        color: #334d50;
        font-weight: normal;
    }
</style>
