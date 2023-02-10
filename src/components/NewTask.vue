<template>
  <div class="container-task">
    <h1>Añade una nueva tarea</h1>
    <div v-if="showErrorMessage">
      <p class="error-text">{{ errorMessage }}</p>
    </div>
    <div class="container-task2">
      <div class="input-field">
        <input
          type="text"
          placeholder="Añade el título de la tarea"
          v-model="name"
        />
      </div>
      <div class="input-field">
        <input
          type="text"
          placeholder="Añade la descripción de la tarea"
          v-model="description"
        />
      </div>
      <button @click="addTask" class="button">Añadir</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task";
import { supabase } from "../supabase";

const taskStore = useTaskStore();

// variables para los valors de los inputs
const name = ref("");
const description = ref("");

// constant to save a variable that holds an initial false boolean value for the errorMessage container that is conditionally displayed depending if the input field is empty
const showErrorMessage = ref(false);

// const constant to save a variable that holds the value of the error message
const errorMessage = ref(null);
const emit = defineEmits(["addTitle"]);

// Arrow function para crear tareas.
const addTask = () => {
  if (name.value.length < 4 || description.value.length < 4) {
    // Primero comprobamos que ningún campo del input esté vacío y lanzamos el error con un timeout para informar al user.

    showErrorMessage.value = true;
    errorMessage.value =
      "Te falta introducir un título y descripción de tu tarea o es muy corto";
    setTimeout(() => {
      showErrorMessage.value = false;
    }, 5000);
  } else {
   // Aquí mandamos los valores a la store para crear la nueva Task. Esta parte de la función tenéis que refactorizarla para que funcione con emit y el addTask del store se llame desde Home.vue.
    taskStore.addTask(name.value, description.value);
    name.value = '';
    description.value = '';
    //Función que hace que se refresque la lista de tareas.
    
    //Aquí hacemos el emit. Home recibirá el emit y llamará a la función getTask(). 
    emit("addTitle") 
  }
};
</script>

<style>
.container-task {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 5vw;
  border: solid 1px red;
}

.container-task2 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  border: solid 1px red;
}

.container-task h1 {
  margin-top: 5vw;
}
.error-text {
  font-size: 0.8vw;
}

.input-field input {
  width: 75vw;
  height: 5vh;
  margin-top: 2.5vh;
  margin-bottom: 2.5vh;
  border-radius: 1vw;
  border-width: 0.1vw;
  padding-left: 2vw;
}

.button {
  margin-top: 2.5vh;
  height: 5vh;
}

.input-field:last-of-type input {
  height: 15vh;
}
</style>
  