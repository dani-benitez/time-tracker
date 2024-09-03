<template>
  <div id="app">
    <header>
      <nav>
        <ul class="flex justify-center space-x-6 bg-gray-800 py-4 text-white">
          <li 
            @click="currentView = 'TimeTracker'" 
            :class="{ 'text-blue-500': currentView === 'TimeTracker' }" 
            class="cursor-pointer hover:text-blue-400"
          >
            Time Tracker
          </li>
          <li 
            @click="currentView = 'TaskLogs'" 
            :class="{ 'text-blue-500': currentView === 'TaskLogs' }" 
            class="cursor-pointer hover:text-blue-400"
          >
            Task Logs
          </li>
          <li 
            @click="currentView = 'ProjectSummary'" 
            :class="{ 'text-blue-500': currentView === 'ProjectSummary' }" 
            class="cursor-pointer hover:text-blue-400"
          >
            Project Summary
          </li>
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
    ProjectSummary
  },
  data() {
    return {
      currentView: 'TimeTracker',
    };
  },
};
</script>

<style scoped>
header {
  background-color: #1f2937; /* Tailwind's bg-gray-800 */
  padding: 10px;
}
nav ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
  display: flex;
  gap: 20px;
}
nav li {
  cursor: pointer;
  transition: color 0.3s;
}
</style>
