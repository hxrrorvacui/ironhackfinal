<template>
  <div class="wrapper">
    <Nav />

    <div class="content">
      <h2>your new task:</h2>
      <!-- <router-link to="/account">Account</router-link> -->
    </div>
    <NewTask  @new-task-emit="addTaskSupabase" />
    <h2>registered tasks:</h2>
    <div class="taskContainer">
    <TaskItem @delete-emit="deleteTask" @update-emit="addUpdateTaskSupabase" v-for="task in tasks" :key="task.id" :task="task" />
    </div>
  </div>
  <!-- <p v-for="task in taskStore.tasksArr" :key="task.id">{{ task }}</p> -->
</template>

<script setup>
import { ref, onUpdated } from "vue";
import { useTaskStore } from "../stores/task";
import { useRouter } from "vue-router";
import Nav from "../components/Nav.vue";
import NewTask from "../components/NewTask.vue";
import TaskItem from "../components/TaskItem.vue";

const taskStore = useTaskStore();

// Variable para guardar las tareas de supabase
const tasks = ref([]);

// Creamos una función que conecte a la store para conseguir las tareas de supabase
const getTasks = async () => {
  tasks.value = await taskStore.fetchTasks()
};
getTasks();

onUpdated(()=> {
  getTasks()
 });

 //testing emit with diego, to test add @test-emit="miCoolFunction" to <NewTask ...> in template & uncomment line 15 in newTask.vue => <!-- <button @click="testFunction">test emit</button> -->
// const miCoolFunction = (miInfoRecibidaEjemplo) => {
//   alert(`Hola ${miInfoRecibidaEjemplo}`);
//  };
// deletetask constant for deleteEmit
const deleteTask = ()=>{
  
console.log("testinggg");
}

const updateTask = ()=>{
  
  console.log("testing modify");
  }
// function to send tasks to supabase
const addTaskSupabase = (newTask) => {
  alert(`${newTask.title}
 ${newTask.description}`);
 // variables para guardar cara clave/key+valor del objeto del emit dentro se su variable correspondiente para poder pasarle segun la logica de la funcion que se conecta con la base de datos en la tienda de task.js con nombre addTask
 let newTaskTitle = newTask.title;
 let newTaskDescription = newTask.description;
 let newTaskStatus = newTask.task_status;
 taskStore.addTask(newTaskTitle, newTaskDescription, newTaskStatus)
//  getTasks();
//  console.log("click");
};
// function to send updated task to supabase
const addUpdateTaskSupabase = (newUpdate) => {
  console.log(newUpdate);
  alert(`${newUpdate.title} ${newUpdate.description}`);
  taskStore.updateTask(newUpdate.title, newUpdate.description, newUpdate.task_status, newUpdate.id)
  
};
</script>

<style></style>

