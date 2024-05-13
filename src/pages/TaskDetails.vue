<script>
export default {
    data() {
        return {
            task: {},
            newTitle: '',
            newDescription: ''
        }
    },
    created() {
        this.fetchTask();
    },
    methods: {
        async fetchTask() {
            let response = await fetch(`http://localhost:3000/tasks/${this.$route.params.id}`);
            let task = await response.json();
            console.log(task);
            this.task = task;
        },
        async deleteTask(){
            const url = `http://localhost:3000/tasks/${this.$route.params.id}`;
            const response = await fetch(url, {
                method: 'DELETE',
                headers: {
                'Content-Type': 'application/json',
                }
            });
            window.location.href = '/';
            return await response.json();
        },
        async editTask(newTitle, newDescription){
            const editedTask = {
                title: newTitle,
                description: newDescription,
            }

            const url = `http://localhost:3000/tasks/${this.$route.params.id}`;
            await fetch(url, {
                method: 'PATCH',
                headers: {
                'Content-Type': 'application/json'
                },
                body: JSON.stringify(editedTask)
            })
            window.location.href = '/';
        },
    }
}
</script>

<template>
    <p>{{ this.$route.params.id }}</p>
    <h2>{{ this.$route.params.title}}</h2>
    <h2>{{ task.title}}</h2>
    <p>{{ task.description}}</p>

    <input v-model="newTitle" :placeholder="task.title">
    <input v-model="newDescription" :placeholder="task.description"></input>
      
    <button  type="button" @click="deleteTask()">Delete</button>
    <button  type="button" @click="editTask(this.newTitle, this.newDescription )">Edit</button>
</template>

<style>
</style>