<template>
  <div class="container">
    <Header @showTask="ShowTask" :showaddtask="showAddTask" title="Task Tracker" />
    <div v-if="showAddTask">
      <AddTask @add-task="AddNewTask" />
    </div>
    
    <Tasks @toggle-reminder="Toggle" @delete-task="deleteTask" :tasks="tasks" />
    <router-view></router-view>
    <Footer />
  
  </div>
  
  
</template>

<script>

import Header from "./components/Header.vue"
import Tasks from "./components/Tasks.vue"
import AddTask from "./components/AddTask.vue"
import Footer from "./components/Footer.vue"

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
    Footer
    
  },

  methods:{
    async deleteTask(id){
      const res=await fetch(`api/tasks/${id}`,{
        method:'DELETE',
      })
      res.status == 200 ? (this.tasks=this.tasks.filter(a=> a.id!=id)) : alert('Error Deleting Task')

    },
    ShowTask(){
      this.showAddTask=!this.showAddTask
    },
    async AddNewTask(task){
      const res=await fetch("api/tasks",{
        method:'POST',
        headers:{
          'Content-Type' :'application/json'
        },
        body: JSON.stringify(task)
      }) 
      const data=await res.json()
      this.tasks=[...this.tasks,data]
    },
    async Toggle(id){
      const res=await this.fetchtask(id)
      const Task={...res,reminder:!res.reminder}
      const updated = await fetch(`api/tasks/${id}`,{
        method:'PUT',
        headers:{
          'Content-Type':'application/json'
        },
        body: JSON.stringify(Task)
      })
      const data =await updated.json()
      
      this.tasks=this.tasks.map(a => a.id===id ? {...a,reminder: data.reminder} : a)
    },
    async fetchall(){
      const res=await fetch("api/tasks")
      const data =await res.json()
      return data
    },
    async fetchtask(id){
      const res=await fetch(`api/tasks/${id}`)
      const data=await res.json()
      return data
    }

  },
  data(){
        return{
            tasks : [],
            showAddTask: false
        }
    },
  async created(){
    this.tasks=await this.fetchall()
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