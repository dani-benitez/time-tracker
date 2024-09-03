<template>
  <div class="min-h-screen bg-gray-900 text-white p-6">
    <h1 class="text-3xl font-bold mb-6 text-center text-blue-500">Task Logs</h1>

    <div class="mb-4">
      <label class="text-green-500 flex items-center">
        Filtrar por Proyecto:
        <select v-model="selectedProjectFilter" class="bg-gray-800 text-white rounded px-2 py-1 ml-2">
          <option value="">Todos los proyectos</option>
          <option v-for="project in projects" :key="project.name" :value="project.name">{{ project.name }}</option>
        </select>
      </label>
    </div>

    <ul class="space-y-4">
      <li 
        v-for="(task, index) in filteredTasks" 
        :key="index" 
        class="bg-gray-800 p-4 rounded-lg shadow-md"
      >
        <div>
          <span class="font-semibold text-green-500">Proyecto:</span> {{ task.project }}
        </div>
        <div>
          <span class="font-semibold text-green-500">Tarea:</span> {{ task.name }}
        </div>
        <div>
          <span class="font-semibold text-green-500">Tiempo:</span> {{ formatTime(task.time) }}
        </div>
      </li>
      <li v-if="filteredTasks.length === 0" class="text-center text-gray-500">
        No hay tareas registradas.
      </li>
    </ul>

    <div class="mt-6 text-right text-white">
      Total de Tareas: {{ filteredTasks.length }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loggedTasks: [],
      selectedProjectFilter: '',
      projects: []
    };
  },
  computed: {
    filteredTasks() {
      return this.getFilteredTasks();
    }
  },
  methods: {
    getFilteredTasks() {
      let tasks = [...this.loggedTasks];  // Clonar el array para evitar problemas de referencia

      if (this.selectedProjectFilter) {
        tasks = tasks.filter(task => task.project === this.selectedProjectFilter);
      }

      return tasks.reverse(); // Mostrar las tareas más recientes primero
    },
    loadTasks() {
      const storedTasks = localStorage.getItem('loggedTasks');
      if (storedTasks) {
        this.loggedTasks = JSON.parse(storedTasks);
      }
    },
    loadProjects() {
      const storedProjects = localStorage.getItem('projects');
      if (storedProjects) {
        this.projects = JSON.parse(storedProjects);
      }
    },
    formatTime(seconds) {
      const h = Math.floor(seconds / 3600).toString().padStart(2, '0');
      const m = Math.floor((seconds % 3600) / 60).toString().padStart(2, '0');
      const s = (seconds % 60).toString().padStart(2, '0');
      return `${h}:${m}:${s}`;
    }
  },
  mounted() {
    this.loadTasks();
    this.loadProjects();
  }
};
</script>
