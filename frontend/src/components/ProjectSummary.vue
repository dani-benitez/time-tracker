<template>
  <div class="min-h-screen bg-gray-900 text-white p-6">
    <h1 class="text-3xl font-bold mb-6 text-center text-blue-500">Project Summary</h1>
    <ul class="space-y-4">
      <li 
        v-for="(project, index) in projectSummary" 
        :key="index" 
        class="bg-gray-800 p-4 rounded-lg shadow-md"
      >
        <div>
          <span class="font-semibold text-green-500">Project:</span> {{ project.name }}
        </div>
        <div>
          <span class="font-semibold text-green-500">Total Time:</span> {{ formatTime(project.totalTime) }}
        </div>
      </li>
      <li v-if="projectSummary.length === 0" class="text-center text-gray-500">
        No projects recorded yet.
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      projectSummary: []
    };
  },
  mounted() {
    this.calculateProjectSummary();
  },
  methods: {
    calculateProjectSummary() {
      const storedTasks = localStorage.getItem('loggedTasks');
      if (storedTasks) {
        const loggedTasks = JSON.parse(storedTasks);
        const projectTimes = {};

        loggedTasks.forEach(task => {
          if (!projectTimes[task.project]) {
            projectTimes[task.project] = 0;
          }
          projectTimes[task.project] += task.time;
        });

        this.projectSummary = Object.keys(projectTimes).map(project => ({
          name: project,
          totalTime: projectTimes[project]
        }));
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
