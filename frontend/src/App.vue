<template>
  <div id="app">
    <header>
      <nav class="bg-gray-800 p-4">
        <ul class="flex justify-center space-x-6 text-white">
          <li @click="currentView = 'TimeTracker'" :class="{ 'text-blue-500': currentView === 'TimeTracker' }" class="cursor-pointer hover:text-blue-400">Time Tracker</li>
          <li @click="currentView = 'TaskLogs'" :class="{ 'text-blue-500': currentView === 'TaskLogs' }" class="cursor-pointer hover:text-blue-400">Task Logs</li>
          <li @click="currentView = 'ProjectSummary'" :class="{ 'text-blue-500': currentView === 'ProjectSummary' }" class="cursor-pointer hover:text-blue-400">Project Summary</li>
          <li @click="currentView = 'ProjectList'" :class="{ 'text-blue-500': currentView === 'ProjectList' }" class="cursor-pointer hover:text-blue-400">Projects</li>
        </ul>
      </nav>
    </header>

    <component :is="currentView" />
  </div>
</template>

<script>
import TimeTracker from './components/TimeTracker.vue';
import TaskLogs from './components/TaskLogs.vue';
import ProjectSummary from './components/ProjectSummary.vue';
import ProjectList from './components/ProjectList.vue';

export default {
  data() {
    return {
      currentView: 'TimeTracker'
    };
  },
  components: {
    TimeTracker,
    TaskLogs,
    ProjectSummary,
    ProjectList
  }
};
</script>
