<template>
  <div class="min-h-screen bg-gray-900 text-white p-6">
    <h1 class="text-3xl font-bold mb-6 text-center text-blue-500">Task Logs</h1>
    <ul class="space-y-4">
      <li 
        v-for="(task, index) in loggedTasks" 
        :key="index" 
        class="bg-gray-800 p-4 rounded-lg shadow-md"
      >
        <div>
          <span class="font-semibold text-green-500">Project:</span> {{ task.project }}
        </div>
        <div>
          <span class="font-semibold text-green-500">Task:</span> {{ task.name }}
        </div>
        <div>
          <span class="font-semibold text-green-500">Time:</span> {{ formatTime(task.time) }}
        </div>
      </li>
      <li v-if="loggedTasks.length === 0" class="text-center text-gray-500">
        No tasks recorded yet.
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loggedTasks: []
    };
  },
  mounted() {
    this.loadLoggedTasks();
  },
  methods: {
    loadLoggedTasks() {
      const storedTasks = localStorage.getItem('loggedTasks');
      if (storedTasks) {
        this.loggedTasks = JSON.parse(storedTasks);
      }
    },
    formatTime(seconds) {
      const hrs = Math.floor(seconds / 3600);
      const mins = Math.floor((seconds % 3600) / 60);
      const secs = seconds % 60;
      return `${hrs.toString().padStart(2, '0')}:${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
    }
  }
};
</script>
