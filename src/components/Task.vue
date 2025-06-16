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
    <InputUI v-model="utask" label="Enter your task" id="utask" size="m" variant="main"/>
    <br>
    <ButtonUI @klick = "submitBtn()" label = "Submit" class="main-var"/>
   <br>
   <br>
    <table>
    <tr>
        <th>Task</th>
        <th>Options</th>
      </tr>
   </table>
    <table id="dTask">
    </table>
    <table>
      <tr>
        <th>Task count</th>
        <th></th>
      </tr>
    </table>
    <br>
    <h3>Filter and Delete Tasks</h3>
    <br>
    <label>Filter by name: </label>
    <InputUI v-model="filterValue" label="Filter by name" id="filterValue" size="s" variant="main"/>
    <ButtonUI @klick = "filterBtn()" label = "Filter" class="main-var"/>
    <br>
    <br>
    <label>Delete by name: </label>
    <InputUI v-model="deleteValue" label="Delete by name" id="deleteValue" size="s" variant="main"/>
    <ButtonUI @klick="deleteBtn()" label="Delete" class="main-var" />
    <p id="confirm" v-show = "confirmation">{{ delconfmsg }}</p>
    <footer>Done by MITWORK 2025</footer>
  </div>
</template>

<script>
import ButtonUI from './ButtonUI.vue'; 
import InputUI from './InputUI.vue';

export default {
  components: {
      ButtonUI,
      InputUI
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

     tasks.forEach((task, index) => {
         const tr = document.createElement('tr');

         const tdTask = document.createElement('td');
         tdTask.textContent = task;
         tr.appendChild(tdTask);

         const tdDelete = document.createElement('td');
         const deleteBtn = document.createElement('button');
         deleteBtn.textContent = 'Delete';
         deleteBtn.classList.add('delete-btn');
         deleteBtn.addEventListener('click', () => this.deleteTask(index));
         tdDelete.appendChild(deleteBtn);
         tr.appendChild(tdDelete);

         taskList.appendChild(tr);
     });

     this.taskData = tasks; 
  },

  deleteTask(index) {
    this.taskData.splice(index, 1);
    localStorage.setItem('taskData', JSON.stringify(this.taskData));
    this.displayTasks();
    this.delconfmsg = 'Task deleted successfully';
    this.confirmation = true;
  },

  filterBtn() {
      const filterValue = document.getElementById('filterValue').value.toLowerCase();
      const taskList = document.getElementById('dTask');
      taskList.innerHTML = ''; 
      this.taskData.forEach((task, index) => {
          if (task.toLowerCase().includes(filterValue)) {
              const tr = document.createElement('tr');

              const tdTask = document.createElement('td');
              tdTask.textContent = task;
              tr.appendChild(tdTask);

              const tdDelete = document.createElement('td');
              const deleteBtn = document.createElement('button');
              deleteBtn.textContent = 'Delete';
              deleteBtn.classList.add('delete-btn');
              deleteBtn.addEventListener('click', () => this.deleteTask(index));
              tdDelete.appendChild(deleteBtn);
              tr.appendChild(tdDelete);

              taskList.appendChild(tr);
          }
      });

      if (taskList.children.length === 0) {
          taskList.innerHTML = '<tr><td colspan="2">No tasks found</td></tr>';
      }
  },

  deleteBtn() {
      const deleteValue = document.getElementById('deleteValue').value.toLowerCase();
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
    margin-right: 10%;
    margin-left: 10%;
    width: 80%;
    padding: 10px;
    transition: background-color 0.3s;
}
table{
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
</style>
