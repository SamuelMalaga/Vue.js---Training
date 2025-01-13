<script>
// ----------------------- With Options API
import axios from 'axios';
export default{
  data() {
    return{
      name: 'John Doe',
      status: 'active',
      tasks: ['Task One', 'Task two', 'Task Three'],
      link: 'https://google.com',
      envVar: import.meta.env.VITE_APP_TESTGENERALVAR,
      envSpecificVar: import.meta.env.VITE_EXECAMB,
      newTask: ''
    }
  },
  methods:{
    toggleStatus(){
      if (this.status ==='active'){
        this.status = 'pending'
      } else if (this.status==='pending'){
        this.status = 'inactive'
      } else {
        this.status = 'active'
      }
    },
    addTask(){
      if(this.newTask.trim() !==''){
        this.tasks.push(this.newTask);
        this.newTask = '';
      }
    },
    deleteTask(index){
      this.tasks.splice(index,1);
    }
  },
  mounted() {
    axios
      .get('https://jsonplaceholder.typicode.com/todos')
      .then((response) => {
        console.log(response.data)
        const data = response.data;
        this.tasks = data.map((task) => task.title);
      })
      .catch((error)=>{
        console.log(error.message)
      });
      
  }
}

// ----------------------- With composition API
// import { ref } from 'vue';


// export default{
//   setup() {
//     const name = ref('John Doe');
//     const status = ref('active');
//     const tasks = ref(['Task One', 'Task two', 'Task Three']);
//     const envVar= import.meta.env.VITE_APP_TESTGENERALVAR;
//     const envSpecificVar= import.meta.env.VITE_EXECAMB

//     const toggleStatus = () => {
//       if (status.value ==='active'){
//         status.value = 'pending'
//       } else if (status.value==='pending'){
//         status.value = 'inactive'
//       } else {
//         status.value = 'active'
//       }
//     }

//     return {
//       name,
//       status,
//       tasks,
//       envVar,
//       envSpecificVar,
//       toggleStatus
//     }
//   }
// }

</script>


<template>
  <h1>VUE APPLICATION {{ name }}</h1>
  <p v-if="status === 'active'"> User is active </p>
  <p v-else-if="status === 'pending'"> User is pendind </p>
  <p v-else>User is inactive</p>
  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task,index) in tasks" :key="task"> 
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask(index)">X</button>
    </li>
  </ul>
  <a v-bind:href="link"> Click for google </a>
  <p>Env var test (VÁRIAVEL GERAL): {{envVar}}</p>
  <p>Ambiente de execução: {{ envSpecificVar }} </p>
  <button v-on:click="toggleStatus">
    Change Status
  </button>
  <button @click="toggleStatus">
    Change Status
  </button>
  <form @submit.prevent="addTask">
    <label for="newTask"> Add Task </label>
    <input type="text" name="newTask" id="newTask" v-model="newTask"/>
    <button type="submit">Submit</button>
  </form>
</template>

