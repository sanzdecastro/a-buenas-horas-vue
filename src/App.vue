<script >
export default {
  name: "App",
  data() {
    return {
      tasks: [],
      categories: [],

      taskTitle: '',
      taskDescription: '',

      activeTask: '',

      newTitle: '',
      newDescription: ''
    }
    
  },
  async created() {
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
  methods: {
    openTask(id) {
      let selectedTask = this.tasks.forEach(task => {
        if (task.id === id) {
          this.activeTask = task;
        }
      });
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
    async deleteTask(id){
      const url = `http://localhost:3000/tasks/${id}`;
      const response = await fetch(url, {
         method: 'DELETE',
         headers: {
           'Content-Type': 'application/json',
         }
      });
      window.location.reload();
      return await response.json();
    },
    async editTask(id, newTitle, newDescription){
      const editedTask = {
        title: newTitle,
        description: newDescription,
      }

      const url = `http://localhost:3000/tasks/${id}`;
      const response = await fetch(url, {
        method: 'PATCH',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(editedTask)
      })
      window.location.reload();
    },
  }
}
</script>

<template>
  <div class="bodyView">
    <h1>A buenas horas</h1>

    <form @submit.prevent="submitTask()">
      <input id="taskTitle" type="text" placeholder="Título de la tarea" v-model="taskTitle"></input>
      <input id="taskDescription" type="text" placeholder="Descripción" v-model="taskDescription"></input>
      <button type="button" @click="submitTask(this.taskTitle, this.taskDescription)">Submit tarea</button>
    </form>

    <div class="tasks-container">
      <div class="tasks" v-if="this.tasks.length > 0" v-for="task in this.tasks">
        <div class="task" @click="openTask(task.id)">
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
          <button  type="button" @click="deleteTask(task.id)">Delete</button>
        </div>
      </div>
      <p v-else>No hay tareas, crea una</p>
    </div>

    <div v-if="this.activeTask != ''" class="singleTask" :id=[this.activeTask.id]>
      <h2>{{ this.activeTask.title}}</h2>
      <h2>{{ this.activeTask.priority}}</h2>
      <p>{{ this.activeTask.description}}</p>

      <input v-model="newTitle" :placeholder="this.activeTask.title">
      <input v-model="newDescription" :placeholder="this.activeTask.description"></input>
      
      <button  type="button" @click="deleteTask(this.activeTask.id)">Delete</button>
      <button  type="button" @click="editTask(this.activeTask.id, this.newTitle, this.newDescription )">Edit</button>
    </div>
  </div>
</template>

<style>




</style>
