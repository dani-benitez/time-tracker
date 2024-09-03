<template>
  <div class="flex items-center justify-center h-[calc(100vh-64px)] bg-gray-900 text-white">
    <!-- Columna Izquierda -->
    <div class="mr-10">
      <div class="mb-4">
        <label class="text-green-500 flex items-center">
          Project:<span class="ml-2"></span>
          <div class="relative inline-block text-left flex items-center ml-2">
            <input 
              v-model="selectedProject" 
              @keydown.enter="createOrSelectProject"
              class="text-white bg-transparent border-b-2 border-gray-600 focus:border-green-500 outline-none cursor-pointer" 
              placeholder="Select or create project..." 
              @focus="showDropdown = true" 
              @blur="hideDropdown"
            />
            <!-- Ícono de Chevron -->
            <svg class="w-4 h-4 ml-2 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
            </svg>
            <!-- Lista desplegable -->
            <ul v-show="showDropdown" class="absolute left-0 mt-2 w-full bg-gray-800 border border-gray-600 rounded-md shadow-lg max-h-48 overflow-y-auto">
              <li v-for="project in filteredProjects" :key="project" @click="selectProject(project)" class="px-4 py-2 hover:bg-gray-700 cursor-pointer">{{ project }}</li>
            </ul>
          </div>
        </label>
      </div>

      <div class="mb-4">
        <label class="text-green-500 flex items-center">
          Task:
          <input 
            v-model="taskName" 
            class="text-white bg-transparent border-b-2 border-gray-600 focus:border-green-500 outline-none ml-2" 
            placeholder="Enter task description..." 
          />
        </label>
      </div>

      <div class="mb-6">
        <h3 class="text-green-500">Previous Tasks</h3>
        <ul>
          <li 
            v-for="(task, index) in previousTasks" 
            :key="index" 
            :class="getTaskOpacityClass(index)"
            class="mb-2"
          >
            {{ task.name }}
          </li>
          <li v-if="previousTasks.length === 0" class="text-gray-500">
            Aún no hay registros.
          </li>
        </ul>
      </div>
    </div>

    <!-- Columna Derecha - Temporizador -->
    <div class="bg-gray-800 p-6 rounded-lg shadow-lg">
      <div class="text-6xl mb-4">
        <span :class="{ 'text-green-500': elapsedHours > 0 }">{{ formattedHours }}</span><span>:</span>
        <span :class="{ 'text-blue-500': elapsedMinutes > 0 }">{{ formattedMinutes }}</span><span>:</span>
        <span :class="{ 'text-red-500': elapsedSeconds > 0 }">{{ formattedSeconds }}</span>
      </div>

      <div class="grid grid-cols-12 gap-2 mb-4">
        <div v-for="n in 12" :key="'hours-' + n" class="h-2 w-2 rounded-full" :class="hourDotClass(n)"></div>
        <div v-for="n in 12" :key="'minutes-' + n" class="h-2 w-2 rounded-full mt-1" :class="minuteDotClass(n)"></div>
        <div v-for="n in 12" :key="'seconds-' + n" class="h-2 w-2 rounded-full mt-1" :class="secondDotClass(n)"></div>
      </div>

      <div class="flex justify-between">
        <button 
          :disabled="!canStartTimer" 
          @click="startTracking"
          :class="{'bg-green-600': canStartTimer, 'bg-gray-600': !canStartTimer }"
          class="px-4 py-2 rounded text-white">
          Start
        </button>
        <button 
          @click="stopTracking" 
          class="bg-red-600 px-4 py-2 rounded text-white">
          Stop
        </button>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      selectedProject: '',
      taskName: '',
      previousTasks: [],  // Aquí se almacenarán las tareas previas del proyecto seleccionado
      showDropdown: false,
      timer: null,
      elapsedTime: 0,
      projects: [], // Aquí se almacenarán los proyectos
      loggedTasks: [] // Aquí se almacenarán las tareas registradas
    };
  },
  computed: {
    canStartTimer() {
      return this.selectedProject && this.taskName;
    },
    elapsedHours() {
      return Math.floor(this.elapsedTime / 3600);
    },
    elapsedMinutes() {
      return Math.floor((this.elapsedTime % 3600) / 60);
    },
    elapsedSeconds() {
      return this.elapsedTime % 60;
    },
    formattedHours() {
      return String(this.elapsedHours).padStart(2, '0');
    },
    formattedMinutes() {
      return String(this.elapsedMinutes).padStart(2, '0');
    },
    formattedSeconds() {
      return String(this.elapsedSeconds).padStart(2, '0');
    },
    filteredProjects() {
      return this.projects.filter(project => 
        project.toLowerCase().includes(this.selectedProject.toLowerCase())
      );
    }
  },
  methods: {
    createOrSelectProject() {
      if (!this.projects.includes(this.selectedProject)) {
        this.projects.push(this.selectedProject);
        this.saveProjects();
      }
      this.showDropdown = false;
    },
    selectProject(project) {
      this.selectedProject = project;
      this.showDropdown = false;
      this.loadPreviousTasks();
    },
    hideDropdown() {
      setTimeout(() => { this.showDropdown = false; }, 200); 
    },
    loadPreviousTasks() {
      // Cargar las 5 tareas previas más recientes
      this.previousTasks = this.loggedTasks.filter(task => task.project === this.selectedProject).slice(-5).reverse();
    },
    startTracking() {
      if (!this.canStartTimer) {
        this.showToast(this.selectedProject ? "Por favor escribe tu tarea" : "Por favor selecciona o crea un proyecto");
        return;
      }
      if (!this.timer) {
        this.timer = setInterval(() => {
          this.elapsedTime += 1;
        }, 1000);
      }
    },
    stopTracking() {
      clearInterval(this.timer);
      this.timer = null;

      // Guardar la tarea registrada
      if (this.taskName && this.elapsedTime > 0) {
        this.loggedTasks.push({
          project: this.selectedProject,
          name: this.taskName,
          time: this.elapsedTime
        });
        this.saveTasks();
      }

      // Reiniciar
      this.taskName = '';
      this.elapsedTime = 0;
    },
    saveProjects() {
      localStorage.setItem('projects', JSON.stringify(this.projects));
    },
    saveTasks() {
      localStorage.setItem('loggedTasks', JSON.stringify(this.loggedTasks));
    },
    loadProjects() {
      const storedProjects = localStorage.getItem('projects');
      if (storedProjects) {
        this.projects = JSON.parse(storedProjects);
      }
    },
    loadTasks() {
      const storedTasks = localStorage.getItem('loggedTasks');
      if (storedTasks) {
        this.loggedTasks = JSON.parse(storedTasks);
      }
    },
    getTaskOpacityClass(index) {
      // Devuelve una clase de Tailwind CSS según la posición de la tarea
      const opacityLevels = [
        'text-white', // 100%
        'text-white opacity-80', // 80%
        'text-white opacity-60', // 60%
        'text-white opacity-40', // 40%
        'text-white opacity-20'  // 20%
      ];
      return opacityLevels[index] || 'text-white opacity-20'; // Fallback a 20% si hay más de 5 tareas
    },
    hourDotClass(n) {
      return { 'bg-green-500': n <= this.elapsedHours, 'bg-gray-600': n > this.elapsedHours };
    },
    minuteDotClass(n) {
      return { 'bg-blue-500': n <= this.elapsedMinutes / 5, 'bg-gray-600': n > this.elapsedMinutes / 5 };
    },
    secondDotClass(n) {
      return { 'bg-red-500': n <= this.elapsedSeconds / 5, 'bg-gray-600': n > this.elapsedSeconds / 5 };
    },
    showToast(message) {
      // Muestra una alerta tipo "toast" con el mensaje proporcionado.
      alert(message); // Podrías implementar un toast más elegante en lugar de `alert`.
    }
  },
  mounted() {
    this.loadProjects();
    this.loadTasks();
  }
};
</script>
