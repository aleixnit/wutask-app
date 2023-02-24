<template>
  <div class="wrapper">
    <Nav />
    <NewTask @addTitle="getTasks" />
    <!-- <h1>Tareas:</h1> -->

    <!-- Cajitas de las task abajo -->
    <div class="boxflex">
      <TaskItem
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        @updateTask="getTasks"
        @childComplete="completeTaskSupabase"
      />
    </div>

    <div class="scroll-to-top" @click="scrollToTop">
      <button class="button4">
        <span class="button_lg">
          <span class="button_sl"></span>
          <span class="button_text">Volver al Inicio</span>
        </span>
      </button>
    </div>

    <FooterComp />
  </div>
</template>

<script setup>
import { ref, onUpdated } from "vue";
import { useTaskStore } from "../stores/task";
import { useRouter } from "vue-router";
import Nav from "../components/Nav.vue";
import NewTask from "../components/NewTask.vue";
import TaskItem from "../components/TaskItem.vue";
import FooterComp from "../components/FooterNew.vue";

const taskStore = useTaskStore();

// Variable para guardar las tareas de supabase
const tasks = ref([]);

// Creamos una función que conecte a la store para conseguir las tareas de supabase
const getTasks = async () => {
  tasks.value = await taskStore.fetchTasks();
};

getTasks();

onUpdated(() => {
  getTasks();
});

const completeTaskSupabase = async (taskObject) => {
  console.log("click");
  console.log(taskObject);
  console.log(taskObject.id);
  console.log(taskObject.is_complete);

  let changeTaskBooleanValue = !taskObject.is_complete;

  let taskId = taskObject.id;

  await taskStore.completeTask(changeTaskBooleanValue, taskId);
};
</script>

<script>
export default {
  methods: {
    scrollToTop() {
      window.scrollTo({
        top: 0,
        behavior: "smooth",
      });
    },
  },
};
</script>
<style>
.scroll-to-top {
  height: 7.5vh;
  background-image: linear-gradient(rgb(232,179,165, 0.7), rgba(242, 71, 24, 0.5)), url("../../assets/fondotaskitem2.png");
  background-size: cover;
  background-attachment: fixed;
  padding-left: 43.5vw;
  padding-top: 0.3vw;
  padding-bottom: 0.3vw;
}

.scroll-to-top h1 {
  font-size: 1vw;
  color: rgb(255, 255, 255);
}

.button4 {
  -moz-appearance: none;
  -webkit-appearance: none;
  appearance: none;
  border: none;
  background: none;
  color: #0f1923;
  cursor: pointer;
  position: relative;
  padding: 8px;
  margin-bottom: 20px;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 14px;
  transition: all .15s ease;
}

.button4::before,
.button4::after {
  content: '';
  display: block;
  position: absolute;
  right: 0;
  left: 0;
  height: calc(50% - 5px);
  border: 1px solid #ffffff;
  transition: all .15s ease;
}

.button4::before {
  top: 0;
  border-bottom-width: 0;
}

.button4::after {
  bottom: 0;
  border-top-width: 0;
}

.button4:active,
.button4:focus {
  outline: none;
}

.button4:active::before,
.button4:active::after {
  right: 3px;
  left: 3px;
}

.button4:active::before {
  top: 3px;
}

.button4:active::after {
  bottom: 3px;
}

.button_lg {
  position: relative;
  display: block;
  padding: 10px 20px;
  color: #fff;
  background-color: #0f1923;
  overflow: hidden;
  box-shadow: inset 0px 0px 0px 1px transparent;
}

.button_lg::before {
  content: '';
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  width: 2px;
  height: 2px;
  background-color: #0f1923;
}

.button_lg::after {
  content: '';
  display: block;
  position: absolute;
  right: 0;
  bottom: 0;
  width: 4px;
  height: 4px;
  background-color: #0f1923;
  transition: all .2s ease;
}

.button_sl {
  display: block;
  position: absolute;
  top: 0;
  bottom: -1px;
  left: -8px;
  width: 0;
  background-color: #f07a50 ;
  transform: skew(-15deg);
  transition: all .2s ease;
}

.button_text {
  position: relative;
}

.button4:hover {
  color: #0f1923;
}

.button4:hover .button_sl {
  width: calc(100% + 15px);
}

.button4:hover .button_lg::after {
  background-color: #fff;
}

</style>

<!-- 
**Hints**
1. ref() is used here!
2. (NewTask, TaskItem, Footer, Nav) components are used here! 
3. Tasks are going to be contained in an array here!
4. An async function is needed to get all of the tasks stored within the supabase database, this async function's body will 
contain the tasks value which be use to store the fetchTasks method which lives inside the userTaskStore. This function 
needs to be called within the setUp script in order to run within the first instance of this component lifecycle.

5. NewTask component will receive a customEvent on this instance of the homeView that will fire the add-to-do function
6. add-to-do function will receive 2 params/arguments that will tak a taskTitle and a taskDescription and the body of this 
async function will call the taskStore that calls the addTask function from the store that pushes the info of the task to the 
backEnd. This is possible by passing the 2 param/arguments that will be passed by the user from the inputs within the NewTask 
Component. 

7. TaskItem component will loop through the tasks-array that will print an individual instance of an individual TaskItem 
component. TaskItem will receive 3 customEvents on this instance of the homeView. 1 customEvent for toggling the task to show 
either a text or an icon to display if the task is completed or not completed. 1 customEevent for removing/deleting the 
task out of the array. 1 customEvent for editing the task title and description. This function needs to call the function 
mentioned on hint4.


7.1-customEvent will fire an async function that will take in 1 param/argument. On the body of this function the param/argument 
will be used to define 2 constants. 1 of this constants will take care of setting the boolean value to the opposite of the 
value that checks wether this task is_complete. 1 of this constants will take of calling the id of this specific task in 
order to call the right id. 
7.2-customEvent will fire an asynf function that will take in 1 param/argument. This async function's body will be used to 
call the deleteTaskmethod which will take the param/argument's id in order to delete the task. This function needs to call 
the function mentioned on hint4. 
7.3-customEvent will fire an async function that will take in 1 param/argument. this async function's body will be used to 
take in 2 constants. 1 constant will take in the param/argument newValue. 1 constant will be used to get the param/argument 
oldValue id. These 2 constants will be sent to the backend via the useTaskStore which holds an editTask method. This function 
needs to call the function mentioned on hint4.
-->
