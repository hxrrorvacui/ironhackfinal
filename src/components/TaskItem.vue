<template>
    <div class="container taskContainer box1">
        <h3 :class="{completed: isComplete}">{{task.title}}</h3>
        <p :class="{completed: isComplete}">{{ task.description }}</p>
        <p :class="{completed: isComplete}">{{ task.task_status }}</p>
        <div class="taskButtonBox">
        <button @click="deleteTask">Delete</button>
        <button @click="completeTask">Completed</button>
        <button @click="showUpdateForm = true">Modify Task</button>
        </div>
        <div v-if="showUpdateForm">
            <div v-if="showErrorMessage">
                <p class="error-text">{{ errorMessage }}</p>
            </div>
            <div class="input-field">
                <input type="text" placeholder="Change Task Title" v-model="newName">
            </div>
            <div class="input-field">
                <input type="text" placeholder="Change Task Description" v-model="newDescription" @keypress.enter="updateTask">
            </div>
            <div class="task-status-field">
            <label for="taskStatus">What is the status of your task?</label>
            <select name="task-status" id="task-status" v-model="newStatus" @keypress.enter="updateTask">
              <option value="select"></option>
                <option value="ASAP">Finish ASAP</option>
                <option value="In Process">In Process</option>
                <option value="Soon">To-Do</option>
            </select>    
            </div>
            <button @click="updateTask" class="button">send Modify</button>
            <br>  
        </div>
    </div>
    </template>
    
    <script setup>
    import { ref } from 'vue';
    import { useTaskStore } from '../stores/task';
    import { supabase } from '../supabase';
    
    const taskStore = useTaskStore();
    const isComplete = ref(props.task.is_complete);
    const showUpdateForm = ref(false);
    const props = defineProps({
        task: Object,
    });
    //variables to show error if there is no input info when modifying the task
    const errorMessage = ref(null);
    const showErrorMessage = ref(false);
    // variables for the input values
    const newName = ref('');
    const newDescription = ref('');
    const newStatus = ref(''); 
    
    
    // Function to delete the task through the store. The problem we will have here (and in NewTask.vue) is that when we modify the database the changes will not be reflected in the v-for of Home.vue because we are not modifying the tasks variable saved in Home. Use the emit to change this and avoid any page refresh.
    const emit = defineEmits(["deleteEmit", "testEmit", "updateEmit"])
    
    // const testFunction = () => {
    //     emit("testEmit", )
    // }
    
    
    const deleteTask = async() => {
        await taskStore.deleteTask(props.task.id);
        // console.log("test");
        alert(`Your task ${props.task.title} was deleted`)
        emit("deleteEmit")
    };
    
    const completeTask = () => {
        //When called it should modify the style of h3 & p.
        isComplete.value = !isComplete.value;
        //it communicates to supabase, to set the state of the task as completed/not completed
        taskStore.toggleTask(isComplete.value, props.task.id);
    };
    
    //new test
    const updateTask = () => {
        if(newName.value.length === 0 || newDescription.value.length === 0){
        // First we check that no input field is empty and launch the error with a timeout to inform the user.
        showErrorMessage.value = true;
        errorMessage.value = 'The task title or description is empty';
        setTimeout(() => {
        showErrorMessage.value = false;
        }, 5000);
    
    } else {
        const newUpdate = {
            title: newName.value,
            description: newDescription.value,
            task_status: newStatus.value,
            id: props.task.id
        }
        emit("updateEmit", newUpdate);
        newName.value = ""
        newDescription.value = ""
        newStatus.value = ""
        console.log("¨test update");
    
    }
    setTimeout(() => {
     showUpdateForm.value = false;
    }, 5000);
    };

    </script>
    
    <style>
    .completed {
        background-color: pink;
    }
    </style>
    