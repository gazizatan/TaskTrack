<template>
  <div class="task">
    <header>
      <h1>Task Manager</h1>
      <p>{{ msg }}</p>
      <nav>
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
          <li><a href="/task">Task</a></li>
          <li><a href="/login">Login</a></li>
          <li><a href="/register">Register</a></li>
          <li><a href="/logout">Logout</a></li>
        </ul>
      </nav>
    </header>
    <label>Give your task: </label>
    <input
    type="text"
    v-model="utask"
    placeholder="Enter your task"
    label="Enter your task" 
    id="utask"
    >
    <br>
    <button @click = "submitBtn()">Submit</button>
    <ul id="dTask"></ul>
    <br>
    <h3>Filter and Delete Tasks</h3>
    <br>
    <label>Filter by name: </label>
    <input type="text" id="filterValue" placeholder="Filter by name">
    <button @click = "filterBtn()">Filter</button>
    <br>
    <br>
    <label>Delete by name: </label>
    <input type="text" id="deleteValue" placeholder="Delete by name">
    <ButtonUI @click="deleteBtn()" label="Delete" />
    <p id="confirm" v-show = "confirmation">{{ delconfmsg }}</p>
    <footer>Done by MITWORK 2025</footer>
  </div>
</template>

<script>
import ButtonUI from './ButtonUI.vue'; 

export default {
  components: {
      ButtonUI,
    },

  name: 'Task',
  props: {
    msg: String
  },
    data() {
        return {
            delconfmsg: '',
            taskData: [],
            utask: '',
            filterValue: '',
            deleteValue: '',
            confirmation: false,

        };
    },

  methods: {
    submitBtn () {
        const taskInput = document.getElementById('utask').value.trim();
        if (taskInput){
            this.taskData.push(taskInput);
            localStorage.setItem('taskData', JSON.stringify(this.taskData));
            this.displayTasks();
        }
    },
    displayTasks() {
       const taskList = document.getElementById('dTask');
       const tasks = JSON.parse(localStorage.getItem('taskData')) || [];
         taskList.innerHTML = '';
       tasks.forEach(task => {
           const li = document.createElement('li');
           li.textContent = task;
           taskList.appendChild(li);
       });
         this.taskData = tasks; 
    },
    filterBtn() {
        const filterValue = document.getElementById('filterValue').value.toLowerCase();
        const taskList = document.getElementById('dTask');
        taskList.innerHTML = ''; 
        this.taskData.forEach(task => {
            if (task.toLowerCase().includes(filterValue)) {
                const li = document.createElement('li');
                li.textContent = task;
                taskList.appendChild(li);
            }
        });
        if (taskList.children.length === 0) {
            taskList.innerHTML = '<li>No tasks found</li>';
        }

    },
    deleteBtn(msg) {
      console.log('Delete button clicked', msg);
        const deleteValue = document.getElementById('deleteValue').value.toLowerCase();
        this.taskList = document.getElementById('dTask');
        const tasks = JSON.parse(localStorage.getItem('taskData')) || [];
        const filteredTasks = tasks.filter(task => !task.toLowerCase().includes(deleteValue));
        
        if (tasks.length === filteredTasks.length) {
            this.delconfmsg = 'No tasks found to delete';
            this.confirmation = true;
            return;
        }
        
        localStorage.setItem('taskData', JSON.stringify(filteredTasks));
        this.taskData = filteredTasks;
        this.displayTasks();
        this.delconfmsg = 'Task deleted successfully';
        this.confirmation = true;
    },
  },
  mounted() {
    const storedTasks = localStorage.getItem('taskData');
    if (storedTasks) {
      this.taskData = JSON.parse(storedTasks);
      this.displayTasks();
    }
  },
};
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
button{
    background-color: #b8f5fe;
    font-family: verdana;
    font-size: 14px;
    width: 100px;
    height: 50px;
    border-radius: 15px;
}
input{
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
    margin-right: 30%;
    margin-left: 30%;
    width: 40%;
    padding: 10px;
    border-radius: 10px;
    transition: background-color 0.3s;
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
</style>
