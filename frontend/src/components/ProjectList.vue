<template>
  <div class="min-h-screen bg-gray-900 text-white p-6">
    <h1 class="text-3xl font-bold mb-6 text-center text-blue-500">Projects</h1>
    <ul class="space-y-4">
      <li 
        v-for="(project, index) in projects" 
        :key="index" 
        class="bg-gray-800 p-4 rounded-lg shadow-md flex justify-between items-center"
      >
        <div>
          <span class="font-semibold text-green-500">Project:</span> {{ project.name }}
        </div>
        <div>
          <span class="font-semibold text-green-500">Status:</span>
          <select v-model="project.status" @change="saveProjects" class="bg-gray-700 text-white rounded px-2 py-1 ml-2">
            <option value="in process">In Process</option>
            <option value="completed">Completed</option>
          </select>
        </div>
      </li>
      <li v-if="projects.length === 0" class="text-center text-gray-500">
        No projects available.
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'ProjectList',  // Añadido el nombre del componente
  data() {
    return {
      projects: []
    };
  },
  mounted() {
    this.loadProjects();
  },
  methods: {
    loadProjects() {
      const storedProjects = localStorage.getItem('projects');
      if (storedProjects) {
        this.projects = JSON.parse(storedProjects);
      }
    },
    saveProjects() {
      localStorage.setItem('projects', JSON.stringify(this.projects));
    }
  }
};
</script>
