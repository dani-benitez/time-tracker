<template>
  <div>
    <h1>Time Tracker</h1>

    <!-- Selector de proyecto -->
    <div>
      <input
        v-model="newProjectName"
        type="text"
        placeholder="Enter new project name"
      />
      <button @click="createProject">Create Project</button>
    </div>

    <div>
      <select v-model="selectedProject">
        <option value="" disabled>Select a project</option>
        <option v-for="project in projects" :key="project" :value="project">
          {{ project }}
        </option>
      </select>
    </div>

    <!-- Campo de texto para ingresar el nombre de la tarea -->
    <input v-model="taskName" type="text" placeholder="Enter task name" />
    <button @click="startTracking" :disabled="!selectedProject || !taskName">
      Start
    </button>
    <button
      @click="stopTracking"
      :disabled="!selectedProject || !taskName || !timer"
    >
      Stop
    </button>
    <p>Elapsed Time: {{ elapsedTime }} seconds</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      taskName: "", // Nombre de la tarea
      newProjectName: "", // Nombre del nuevo proyecto
      selectedProject: "", // Proyecto seleccionado
      projects: [], // Lista de proyectos
      timer: null, // Identificador del temporizador
      elapsedTime: 0, // Tiempo transcurrido
      loggedTasks: [], // Lista de tareas registradas
    };
  },
  mounted() {
    // Recuperar proyectos y tareas desde localStorage
    this.loadFromLocalStorage();
  },
  methods: {
    loadFromLocalStorage() {
      const storedProjects = localStorage.getItem("projects");
      const storedTasks = localStorage.getItem("loggedTasks");

      if (storedProjects) {
        this.projects = JSON.parse(storedProjects);
      }
      if (storedTasks) {
        this.loggedTasks = JSON.parse(storedTasks);
      }
    },
    saveToLocalStorage() {
      localStorage.setItem("projects", JSON.stringify(this.projects));
      localStorage.setItem("loggedTasks", JSON.stringify(this.loggedTasks));
    },
    createProject() {
      if (this.newProjectName && !this.projects.includes(this.newProjectName)) {
        this.projects.push(this.newProjectName);
        this.selectedProject = this.newProjectName;
        this.newProjectName = "";
        this.saveToLocalStorage();
      }
    },
    startTracking() {
      if (!this.timer && this.selectedProject && this.taskName) {
        this.timer = setInterval(() => {
          this.elapsedTime += 1;
        }, 1000);
      }
    },
    stopTracking() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;

        // Guardar la tarea registrada
        if (this.taskName && this.elapsedTime > 0) {
          this.loggedTasks.push({
            project: this.selectedProject,
            name: this.taskName,
            time: this.elapsedTime,
          });
          this.saveToLocalStorage();
        }

        // Reiniciar el nombre de la tarea y el tiempo transcurrido
        this.taskName = "";
        this.elapsedTime = 0;
      }
    },
  },
};
</script>

<style scoped>
h1 {
  color: #42b983;
}
input {
  margin-bottom: 10px;
}
select {
  margin: 10px 0;
}
</style>
