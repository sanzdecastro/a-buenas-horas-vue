<script >
import TaskDetails from './pages/TaskDetails.vue';
export default {
  name: "App",
  components: {
    TaskDetails,
  },
  data() {
    return {
      tasks: [],
      categories: [],

      taskTitle: '',
      taskDescription: '',
    }
  },
  async created() {
    this.fetchTasks();
  },
  methods: {
    async fetchTasks() {
      try {
        let response = await fetch("http://localhost:3000/tasks");
        let tasks = await response.json();
        console.log(tasks);
        this.tasks = tasks;
      }
      catch(error) {
        console.log(error);
      }
    },
    async submitTask(taskTitle, taskDescription){
      const newTask = {
        title: taskTitle,
        description: taskDescription,
      }
      const url = "http://localhost:3000/tasks";
      const response = await fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(newTask)
      })
      window.location.reload();
    },
  }
}
</script>

<template>
  <nav>
      <ul>
        <router-link to="/home">Home</router-link>
      </ul>
    </nav>
    
  <div class="bodyView">
    <h1>A buenas horas</h1>

    <form @submit.prevent="submitTask()">
      <input id="taskTitle" type="text" placeholder="Título de la tarea" v-model="taskTitle"></input>
      <input id="taskDescription" type="text" placeholder="Descripción" v-model="taskDescription"></input>
      <button type="button" @click="submitTask(this.taskTitle, this.taskDescription)">Submit tarea</button>
    </form>

    <div class="tasks-container">
      <div class="tasks" v-if="this.tasks.length > 0" v-for="task in this.tasks">
        <div class="task">
          <div class="header-task">
            <h2>{{ task.title }}</h2>
            <div class="priority" :class="task.priority">{{ task.priority }}</div>
          </div>
          <div class="date-time">
            <span> {{ task.date }}</span>
            <span> {{ task.time }}</span>
          </div>
          <div class="categories">
            <div class="tag" v-for="category in task.categories">
              {{ category }}
            </div>
          </div>
          <router-link :to="`task/${task.id}`">See details</router-link>
          
        </div>
      </div>
      <p v-else>No hay tareas, crea una</p>
    </div>
    <router-view>

    </router-view>
    

  </div>
</template>

<style>




</style>
