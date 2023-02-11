<template>
  <div class="container-task">
    <div class="div1">
      <h1>¡Añade una nueva tarea!</h1>
      <p>Keep your life organized, prepare for a trip ? Start here</p>
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
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 5vw;
  height: 70vh;
  /* padding-top: 5vh; */
  padding-bottom: 5vh;
  padding-right: 10vw;
  padding-left: 10vw;
  background-image: url("../../assets/el-sol-al-amanecer-entre-las-nubes_5120x2880_xtrafondos.com.jpg");
  background-attachment: fixed;
  background-size: cover;
}

.div1 {
  width: 60%;
  /* border: solid 1px red; */
  color: #ffffff;
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
  --hovered-color:#fdc999;
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
  color: var(--primary-color)
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
  color: var(--hovered-color)
}

.button2 svg {
  color: var(--primary-color);
  transition: 0.2s;
  position: relative;
  width: 15px;
  transition-delay: 0.2s;
}







/* .button {
  height: 10vw;
  width: 10vw;
}

.button {
 --border-radius: 15px;
 --border-width: 4px;
 appearance: none;
 position: relative;
 padding: 1em 2em;
 border: 0;
 background: linear-gradient(145deg,#ffffff, #e6e6e6);
 font-family: "Roboto", Arial, "Segoe UI", sans-serif;
 font-size: 18px;
 font-weight: 500;
 color: #48026b;
 z-index: 2;
 border-radius: var(--border-radius);
}

.button::after {
 --m-i: linear-gradient(#000, #000);
 --m-o: content-box, padding-box;
 content: "";
 position: absolute;
 left: 0;
 top: 0;
 width: 100%;
 height: 100%;
 padding: var(--border-width);
 border-radius: var(--border-radius);
 background-image: conic-gradient(
		#488cfb,
		#29dbbc,
		#ddf505,
		#ff9f0e,
		#e440bb,
		#655adc,
		#488cfb
	);
 -webkit-mask-image: var(--m-i), var(--m-i);
 mask-image: var(--m-i), var(--m-i);
 -webkit-mask-origin: var(--m-o);
 mask-origin: var(--m-o);
 -webkit-mask-clip: var(--m-o);
 mask-composite: exclude;
 -webkit-mask-composite: destination-out;
 filter: hue-rotate(0);
 animation: rotate-hue linear 500ms infinite;
 animation-play-state: paused;
}

.button:hover::after {
 animation-play-state: running;
}

@keyframes rotate-hue {
 to {
  filter: hue-rotate(1turn);
 }
}

.button,
.button::after {
 box-sizing: border-box;
}

.button:active {
 --border-width: 5px;
}

.input-field:last-of-type input {
  height: 15vh;
} */
</style>
