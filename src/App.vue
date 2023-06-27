<template>
  <div class="container">
    <Header 
      @toggle-add-task="toggleAddTask" 
      title="Task Tracker"
      :showAddTask="showAddTask">
    </Header>
    <div v-show="showAddTask">
      <!-- <AddTask 
        :taskToEdit="taskToEdit"
        :changeTask="changeTask" 
        @add-task="addTask"
        @edit-task="editTask" /> -->
    </div>
    <Tasks 
      @open-task-editor="openTaskEditor"
      @delete-task="deleteTask" 
      :tasks="tasks"></Tasks>
  </div>
</template>

<script>
import Header from './components/Header'
import Tasks from './components/Tasks'

export default {
  name: 'App',

  components: {
    Header,
    Tasks,
  },

  data() {
    return {
      tasks: [],
      showAddTask: false,
      changeTask: false,
      taskToEdit: {},
    }
  },

  methods: {
    async deleteTask(id) {
      const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: "DELETE"
      })

      if (res.status === 200) {
        this.tasks = this.tasks.filter((task) => task.id != id)
      }
      else {
        alert('Error with deleting the task')
      }
    },
    async fetchTasks() {
      const res = await fetch('http://localhost:5000/tasks')

      const data = await res.json()

      return data
    },

    async fetchTask(id) {
      const res = await fetch(`http://localhost:5000/tasks/${id}`)

      const data = await res.json()

      return data
    }
  },
  
  async created() {
    this.tasks = await this.fetchTasks()

    function compareSecs(a, b) {
    return a.secs - b.secs;
    }

    this.tasks.sort(compareSecs);
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
  max-width: 480px;
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

// CSS Styling Ends
