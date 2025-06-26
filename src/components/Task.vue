<template>
  <div class="task">
    <header>
      <h1>Task Manager</h1>
      <p>{{ msg }}</p>
      <nav>
        <ul>
          <li><a href="#">Home</a></li>
          <li><a href="#">About</a></li>
          <li><a href="#">Contact</a></li>
          <li><a href="#">Task</a></li>
          <li><a href="#">Login</a></li>
          <li><a href="#">Register</a></li>
          <li><a href="#">Logout</a></li>
        </ul>
      </nav>
    </header>
    <div class="task-form">
      <label>Task title: </label>
      <InputUI
        v-model="uTask"
        label="Enter your task"
        placeholder="Enter your task"
        size="m"
        variant="main"
        required
        style="max-width: 220px"
      />
      <label style="margin-left: 10px">Description: </label>
      <InputUI
        v-model="uTaskDesc"
        label="Description"
        placeholder="Description"
        size="m"
        variant="main"
        style="max-width: 220px"
      />
      <ButtonUI
        :loading="loading"
        label="Submit"
        class="main-var"
        size="m"
        style="margin-left: 10px"
        @click="submitBtn"
      />
    </div>
    <p v-show="confirmation" id="confirm">
      {{ delConfMsg }}
    </p>
    <div class="task-table-wrapper">
      <table class="task-table">
        <thead>
          <tr>
            <th>Task</th>
            <th style="width: 180px">
Options
</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in filteredTasks" :key="index">
            <td>{{ task.title }}</td>
            <td>
              <ButtonUI
                label="View"
                class="main-var"
                size="s"
                style="margin-right: 8px"
                @click="openView(index)"
              />
              <ButtonUI label="Delete" class="main-var" size="s" @click="deleteTask(index)" />
            </td>
          </tr>
          <tr v-if="filteredTasks.length === 0">
            <td colspan="2">
No tasks found
</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="task-count">
      <span>Task count: <b>{{ filteredTasks.length }}</b></span>
    </div>
    <div class="task-filter">
      <h3>Filter Tasks</h3>
      <InputUI
        v-model="filterValue"
        label="Filter by name"
        placeholder="Type to filter"
        size="m"
        variant="main"
        type="text"
        style="max-width: 300px"
      />
      <ButtonUI
        label="Clear"
        class="main-var"
        size="s"
        style="margin-left: 10px"
        @click="clearFilter"
      />
    </div>
    <div v-if="showView">
      <div class="view-backdrop" @click="closeView"></div>
      <ViewUI
        :u-task="taskData[viewIndex].title"
        :task-dis="taskData[viewIndex].description"
        :edit-mode="editMode"
        :edit-title.sync="editTask.title"
        :edit-description.sync="editTask.description"
        class="view-popup"
        @close="closeView"
        @edit="startEdit"
        @save-edit="saveEdit"
        @cancel-edit="cancelEdit"
      />
    </div>
    <footer>Done by MITWORK 2025</footer>
  </div>
</template>

<script>
import ButtonUI from './comps/ButtonUI.vue'
import InputUI from './comps/InputUI.vue'
import ViewUI from './comps/ViewUI.vue'

export default {
  name: 'Task',
  components: {
    ButtonUI,
    InputUI,
    ViewUI
  },
  props: {
    msg: {
      type: String,
      default: 'Welcome to your task manager!',
      required: true
    }
  },
  data() {
    return {
      delConfMsg: '',
      taskData: [],
      uTask: '',
      uTaskDesc: '',
      filterValue: '',
      confirmation: false,
      loading: false,
      showView: false,
      viewIndex: null,
      editMode: false,
      editTask: { title: '', description: '' }
    }
  },
  computed: {
    filteredTasks() {
      if (!this.filterValue.trim()) return this.taskData
      return this.taskData.filter((task) =>
        task.title.toLowerCase().includes(this.filterValue.toLowerCase())
      )
    }
  },

  mounted() {
    const storedTasks = localStorage.getItem('taskData')
    if (storedTasks) {
      try {
        const parsed = JSON.parse(storedTasks)
        if (Array.isArray(parsed) && typeof parsed[0] === 'string') {
          this.taskData = parsed.map((t) => ({ title: t, description: '' }))
        } else {
          this.taskData = parsed
        }
      } catch {
        this.taskData = []
      } finally {
        console.log('Task data loaded from localStorage')
      }
    }
  },

  methods: {
    submitBtn() {
      if (this.uTask.trim().length === 0) {
        this.delConfMsg = 'Task cannot be empty'
        this.confirmation = true
        return
      }
      this.loading = true
      setTimeout(() => {
        this.taskData.push({
          title: this.uTask.trim(),
          description: this.uTaskDesc.trim()
        })
        this.saveTasks()
        this.uTask = ''
        this.uTaskDesc = ''
        this.delConfMsg = 'Task added successfully'
        this.confirmation = true
        this.loading = false
      }, 700)
    },

    deleteTask(index) {
      this.taskData.splice(index, 1)
      this.saveTasks()
      this.delConfMsg = 'Task deleted successfully'
      this.confirmation = true
      if (this.showView && this.viewIndex === index) {
        this.closeView()
      }
    },

    openView(index) {
      this.viewIndex = index
      this.showView = true
    },

    closeView() {
      this.showView = false
      this.viewIndex = null
    },

    saveTasks() {
      localStorage.setItem('taskData', JSON.stringify(this.taskData))
    },

    clearFilter() {
      this.filterValue = ''
    },

    startEdit() {
      this.editMode = true
      this.editTask = { ...this.taskData[this.viewIndex] }
    },

    saveEdit({ title, description }) {
      if (title.trim().length === 0) {
        this.delConfMsg = 'Task title cannot be empty'
        this.confirmation = true
        return
      }
      this.$set(this.taskData, this.viewIndex, { title, description })
      this.saveTasks()
      this.editMode = false
      this.closeView()
      this.delConfMsg = 'Task updated successfully'
      this.confirmation = true
    },

    cancelEdit() {
      this.editMode = false
    }
  }
}
</script>
<style>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
button {
  background-color: #b8f5fe;
  font-family: verdana;
  font-size: 14px;
  width: 100px;
  height: 50px;
  border-radius: 15px;
}
input {
  width: 400px;
  height: 50px;
}
#confirm {
  color: red;
  font-weight: bold;
  font-size: 20px;
  margin-top: 20px;
  text-align: center;
}
#dTask {
  border-color: #004589;
  border-style: solid;
  border-width: 1px;
  margin-right: 10%;
  margin-left: 10%;
  width: 80%;
  padding: 10px;
  transition: background-color 0.3s;
}
table {
  border-color: #004589;
  border-style: solid;
  border-width: 1px;
  margin-right: 10%;
  margin-left: 10%;
  width: 80%;
  padding: 10px;
  transition: background-color 0.3s;
}
table tr th {
  padding: 10px;
}
footer {
  margin-top: 20%;
  font-size: 14px;
  color: white;
  text-align: center;
  background-color: #0081ff;
  padding: 40px;
}
header {
  background-color: #0081ff;
  color: white;
  padding: 20px;
  text-align: center;
  margin-bottom: 50px;
}

nav ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
nav ul li {
  display: inline;
  margin-right: 20px;
  border-radius: 20px;
  width: 70%;
}
nav ul li a {
  color: white;
  text-decoration: none;
  padding: 10px 20px;
}
nav ul li a:hover {
  background-color: #004589;
}
.task {
  max-width: 70%;
  margin: 0 auto;
  background: #fafdff;
  border-radius: 16px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.07);
  padding: 32px 24px 24px 24px;
}
.task-form {
  margin-bottom: 18px;
  display: flex;
  align-items: center;
  gap: 10px;
}
.task-table-wrapper {
  overflow-x: auto;
  margin-bottom: 16px;
}
.task-table {
  width: 80%;
  border-collapse: collapse;
  background: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.03);
}
.task-table th,
.task-table td {
  padding: 10px 12px;
  border-bottom: 1px solid #eaeaea;
  text-align: left;
}
.task-table th {
  background: #eaf6fb;
  font-weight: 600;
}
.task-table tr:last-child td {
  border-bottom: none;
}
.task-count {
  margin: 12px 0 24px 0;
  font-size: 16px;
  text-align: right;
}
.task-filter {
  margin-bottom: 24px;
}
.view-popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.18);
  z-index: 1000;
  padding: 32px 24px;
  min-width: 320px;
}
.view-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.25);
  z-index: 1000;
}
</style>
