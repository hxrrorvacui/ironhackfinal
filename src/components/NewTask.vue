<template>
  <div class="box2">
    <h3>Add a new Task</h3>
    <div v-if="showErrorMessage">
        <p class="error-text">{{ errorMessage }}</p>
    </div>
    <div>
        <div class="input-field">
            <input type="text" placeholder="Add a Task Title" v-model="name">
        </div>
        <div class="input-field">
            <input type="text" placeholder="Add a Task Description" v-model="description" @keypress.enter="addTask">
        </div>
        <br><br>
        <div class="task-status-field">
            <label for="taskStatus">What is the status of your task?</label>
            <select name="task-status" id="task-status" v-model="status">
              <option value="select"></option>
                <option value="ASAP">Finish ASAP</option>
                <option value="In Process">In Process</option>
                <option value="Soon">To-Do</option>
            </select>    
        </div>
        <br>
        <button @click="addTask" class="button button-add">Add</button>
        <br><br>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task"   


// constant to save a variable that holds an initial false boolean value for the errorMessage container that is conditionally displayed depending if the input field is empty
const showErrorMessage = ref(false);

// const constant to save a variable that holds the value of the error message
const errorMessage = ref(null);

// --
// --
// --
// --
// --
// --

//  var to save my native method of VUE that is autimported, that is to say I do not have to reference it in "ïmport" as we do with the ref in line 18, this method has this syntax here ^
const emit = defineEmits(["testEmit", "newTaskEmit"])

const testFunction = () => {
    emit("testEmit", name.value)
}

// var to save the use of the ta task store in this file.
const taskStore = useTaskStore();

// variables for input values
const name = ref('');
const description = ref('');
const status = ref('');


// Arrow function to create tasks
function addTask() {
  if(name.value.length===0||description.value.length===0) {
    // First we check that no input field is empty and launch the error with a timeout to inform the user.
    showErrorMessage.value=true;
    errorMessage.value='The task title or description is empty';
    setTimeout(() => {
      showErrorMessage.value=false;
    }, 5000);

  } else {
    const newTask={
      title: name.value,
      description: description.value,
      task_status: status.value,
    };
    emit("newTaskEmit", newTask);
    name.value=""
    description.value=""
    status.value=""

    console.log("testing");
  }
};

</script>

<style></style>