<script>
  import Header from './components/Header.vue';
  import TodoTask from './components/TodoTask.vue';
  export default {
  name: "App",
  components: {
    Header,
    TodoTask
  },
  }
</script>

<template>
  <Header/>
  <TodoTask/>
</template>

<style scoped>
header {
  line-height: 1.5;
}
</style>