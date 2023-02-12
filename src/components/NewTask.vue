<template>
  <div class="container-task">
    <div class="div1">
      <h1>
        ¡Crea y edita tareas! <br />
        La organización es la clave del éxito.
      </h1>
      <p>
        " Tus días están contados. Úsalos para abrir las ventanas de tu alma y
        que entre el sol. <br />
        Si no lo haces, el sol se pondrá en el horizonte y tú con él. " 
        <span style="font-weight: bold"> - Marco Aurelio</span>
      </p>
      <div v-if="showErrorMessage">
        <p class="error-text">{{ errorMessage }}</p>
      </div>
    </div>
    <div class="div2">
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
    </div>
    <div class="div 3">
      <button @click="addTask" class="button2">
        <p>Añadir</p>
        <svg
          stroke-width="4"
          stroke="currentColor"
          viewBox="0 0 24 24"
          fill="none"
          class="h-6 w-6"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M14 5l7 7m0 0l-7 7m7-7H3"
            stroke-linejoin="round"
            stroke-linecap="round"
          ></path>
        </svg>
      </button>
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
    name.value = "";
    description.value = "";
    //Función que hace que se refresque la lista de tareas.

    //Aquí hacemos el emit. Home recibirá el emit y llamará a la función getTask().
    emit("addTitle");
  }
};
</script>

<style>
.container-task {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 2vw;
  height: 90vh;
  width: 100%;
  padding-top: 5vh;
  padding-bottom: 5vh;
  background-image: url("../../assets/el-sol-al-amanecer-entre-las-nubes_5120x2880_xtrafondos.com.jpg");
  background-attachment: fixed;
  background-size: cover;
  position: absolute;
  top: 0;
}

.div1 {
  width: 60%;
  /* border: solid 1px red; */
  color: #ffffff;
  text-align: center;
  padding-top: 3vh;
}

.div1 p {
  line-height: 3, 5vh;
  font-size: 1.3vw;
  font-style: italic;
}

.div2 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  /* border: solid 1px red; */
  /* border: solid 1px red; */
}

.container-task h1 {
  margin-top: 5vw;
}
.error-text {
  font-size: 0.8vw;
}

.input-field input {
  width: 40vw;
  height: 5vh;
  margin-top: 2.5vh;
  margin-bottom: 2.5vh;
  border-radius: 1vw;
  border-width: 0.1vw;
  padding-left: 2vw;
  opacity: 35%;
}

.button2 {
  padding: 0;
  margin: 0;
  border: none;
  background: none;
}

.button2 {
  --primary-color: #ffffff;
  --hovered-color: #fdc999;
  position: relative;
  display: flex;
  font-weight: 600;
  font-size: 3vw;
  gap: 0.5rem;
  align-items: center;
}

.button2 p {
  margin: 0;
  position: relative;
  font-size: 3vw;
  color: var(--primary-color);
}

.button2::after {
  position: absolute;
  content: "";
  width: 0;
  left: 0;
  bottom: -7px;
  background: var(--hovered-color);
  height: 2px;
  transition: 0.3s ease-out;
}

.button2 p::before {
  position: absolute;
  /*   box-sizing: border-box; */
  content: "Añadir";
  width: 0%;
  inset: 0;
  color: var(--hovered-color);
  overflow: hidden;
  transition: 0.3s ease-out;
}

.button2:hover::after {
  width: 100%;
}

.button2:hover p::before {
  width: 100%;
}

.button2:hover svg {
  transform: translateX(4px);
  color: var(--hovered-color);
}

.button2 svg {
  color: var(--primary-color);
  transition: 0.2s;
  position: relative;
  width: 15px;
  transition-delay: 0.2s;
}

</style>
