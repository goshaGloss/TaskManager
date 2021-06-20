<template>
    <div class="container">
        <Header @show_btn = "showBtn"/>
      <div v-if="ShowAddTask">
        <AddTask @add-task="newTask"/>
      </div>
       <Tasks @toggle-reminder = "toggleReminder" @delete-task="deleteTask" :tasks = "tasks" />
       <Footer />
    </div>
</template>

<script>

import Header from './components/Header'
import Footer from './components/Footer'
import Tasks from './components/Tasks'
import AddTask from './components/Addtask'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
    Footer,
  },
  emits:['delete-task','toggle-reminder'],
  data(){
    return{
      tasks: [],
      ShowAddTask:false
    }
  },
  methods:{
    showBtn(){
      this.ShowAddTask = !this.ShowAddTask
    },
    async newTask(task){
      const res = await fetch('api/tasks',{
        method : 'POST',
        headers : {
          'Content-type' : 'application/json'
        },
        body : JSON.stringify(task)
      })

      const data = await res.json()

      this.tasks = [...this.tasks,data]
    },
    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id)
      const updTask = {...taskToToggle, reminder : !taskToToggle.reminder}
      const res = await fetch(`api/tasks/${id}`, {
        method : 'PUT',
        headers : {
          'Content-type' : 'application/json'
        },
        body : JSON.stringify(updTask)
      })
      const data = await res.json()
      this.tasks = this.tasks.map((task)=>task.id === id ? {...task,reminder: data.reminder} : task);
    },
    async deleteTask(id){
      const res = await fetch(`api/tasks/${id}`,{
        method : 'DELETE'
      })
      res.status === 200 ? (this.tasks = this.tasks.filter((task)=>task.id !== id)) : alert('error')
      
    },
    async fetchTasks(){
      const res = await fetch('api/tasks')
      const data = await res.json()
      return data;
    },
    async fetchTask(id){
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data;
    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
