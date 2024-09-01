<template>
  <div>
    <h1>Task Logs</h1>
    <ul>
      <li v-for="(task, index) in loggedTasks" :key="index">
        Project: {{ task.project }}, Task: {{ task.name }}, Time:
        {{ task.time }} seconds
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loggedTasks: [],
    };
  },
  mounted() {
    this.loadLoggedTasks();
  },
  methods: {
    loadLoggedTasks() {
      const storedTasks = localStorage.getItem("loggedTasks");
      if (storedTasks) {
        this.loggedTasks = JSON.parse(storedTasks);
      }
    },
  },
};
</script>

<style scoped>
h1 {
  color: #42b983;
}
</style>
