<template>
  <div>
    <h1>Project Summary</h1>
    <ul>
      <li v-for="(project, index) in projectSummary" :key="index">
        Project: {{ project.name }}, Total Time:
        {{ formatTime(project.totalTime) }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      projectSummary: [],
    };
  },
  mounted() {
    this.calculateProjectSummary();
  },
  methods: {
    calculateProjectSummary() {
      const storedTasks = localStorage.getItem("loggedTasks");
      if (storedTasks) {
        const loggedTasks = JSON.parse(storedTasks);
        const projectTimes = {};

        loggedTasks.forEach((task) => {
          if (!projectTimes[task.project]) {
            projectTimes[task.project] = 0;
          }
          projectTimes[task.project] += task.time;
        });

        this.projectSummary = Object.keys(projectTimes).map((project) => ({
          name: project,
          totalTime: projectTimes[project],
        }));
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
