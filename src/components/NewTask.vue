<template>
  <div class="container-task">
    <div class="div1">
      <h1>
        <div>¡<span class="underlined-text">Crea</span> y <span class="underlined-text">edita</span> tareas! <br /></div>
        <div>La <span class="underlined-text">organización</span> es la clave del éxito.</div>
      </h1>
      <p>
        " Tus días están contados. Úsalos para abrir las ventanas de tu alma y
        que entre el sol. <br />
        Si no lo haces, el sol se pondrá en el horizonte y tú con él. " -
        <span
          style="
            font-weight: bold;
            text-decoration: underline;
            text-decoration-color: #e48970;
          "
        >
          Marco Aurelio</span
        >
      </p>
      <div v-if="showErrorMessage">
        <p class="error-text">{{ errorMessage }}</p>
      </div>
    </div>

    <div class="div2">
      <!-- input animado -->
      <div class="group">
        <input required="" type="text" v-model="name" class="input" />
        <span class="highlight"></span>
        <span class="bar"></span>
        <label>Título de la tarea</label>
      </div>

      <div class="group">
        <input required="" type="text" v-model="description" class="input" />
        <span class="highlight"></span>
        <span class="bar"></span>
        <label>Descripción de la tarea</label>
      </div>
    </div>

    <div class="div 3">
      <button @click="addTask" class="fancy">
        <span class="top-key"></span>
        <span class="text">Añadir tarea</span>
        <span class="bottom-key-1"></span>
        <span class="bottom-key-2"></span>
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

.underlined-text {
  background-image: linear-gradient(to left, #ebc3b8, #e48970);
  background-size: 100% 0.4vw; /* Establece el tamaño del subrayado */
  background-position: 0 95%; /* Establece la posición del subrayado */
  background-repeat: no-repeat; /* Evita que se repita el subrayado */
  border-radius: 1vw;
}

.div1 p {
  line-height: 3, 5vh;
  font-size: 1.3vw;
  font-style: italic;
  text-shadow: -2px 2px 6px rgba(0, 0, 0, 0.55);
}

.div2 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 2vw;
  margin-top: 1.5vw;
  margin-bottom: 1vw;
  /* border: solid 1px red; */
  /* border: solid 1px red; */
}

.container-task h1 {
  margin-top: 5vw;
  display: flex;
  flex-direction: column;
  text-shadow: -2px 2px 6px rgba(0, 0, 0, 0.55);
}
.error-text {
  font-size: 0.8vw;
  background: linear-gradient( #e8b3a5, #e48970);
  border-radius: 2vw;
  text-shadow: -2px 2px 6px rgba(0, 0, 0, 0.55);
  padding-top: 1vh;
  padding-bottom: 1vh;
  margin-top: 3vh;
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
