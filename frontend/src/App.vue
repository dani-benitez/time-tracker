<template>
  <div id="app">
    <header>
      <nav>
        <ul class="flex justify-center">
          <li @click="currentView = 'TimeTracker'">Time Tracker</li>
          <li @click="currentView = 'TaskLogs'">Task Logs</li>
          <li @click="currentView = 'ProjectSummary'">Project Summary</li>
        </ul>
      </nav>
    </header>

    <component :is="currentView" />
  </div>
</template>

<script>
import TimeTracker from "./components/TimeTracker.vue";
import TaskLogs from "./components/TaskLogs.vue";
import ProjectSummary from "./components/ProjectSummary.vue";

export default {
  name: "App",
  components: {
    TimeTracker,
    TaskLogs,
    ProjectSummary,
  },
  data() {
    return {
      currentView: "TimeTracker",
    };
  },
};
</script>

<style scoped>
header {
  background-color: #42b983;
  padding: 10px;
}

nav ul {
  list-style-type: none;
  padding: 0;
  display: flex;
  gap: 20px;
}
nav li {
  cursor: pointer;
  color: white;
}
</style>

<style>
body {
  font-family: "Roboto", sans-serif; /* Reemplaza 'Roboto' con el nombre de tu fuente */
}
</style>
