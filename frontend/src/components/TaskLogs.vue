<template>
  <div>
    <h1>Task Logs</h1>
    <ul>
      <li v-for="(task, index) in loggedTasks" :key="index">
        Project: {{ task.project }}, Task: {{ task.name }}, Time:
        {{ formatTime(task.time) }}
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
    formatTime(seconds) {
      const hrs = Math.floor(seconds / 3600);
      const mins = Math.floor((seconds % 3600) / 60);
      const secs = seconds % 60;
      return `${hrs.toString().padStart(2, "0")}:${mins
        .toString()
        .padStart(2, "0")}:${secs.toString().padStart(2, "0")}`;
    },
  },
};
</script>

<style scoped>
h1 {
  color: #42b983;
}
</style>
