<script >
export default {
  name: "App",
  data() {
    return {
      tasks: []
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

  }
}
</script>

<template>
  <h1>A buenas horas</h1>

  <div class="tasks" v-for="task in tasks">
    <div class="task">
      <div class="header-task">
        <h2>{{ task.title }}</h2>
        <div class="priority">{{ task.priority }}</div>
      </div>
      <div class="date-time">
        <span> {{ task.date }}</span>
        <span> {{ task.time }}</span>
      </div>
      <div class="categories" v-if="task.categories.length !== 0">
        <div class="tag" v-for="category in task.categories">
          {{ category }}
        </div>
      </div>
    </div>
  </div>
 
</template>

<style>
.task {
  @apply
  border-solid
  border-black
  border
  m-3;
}
</style>
