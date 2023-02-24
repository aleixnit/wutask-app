<template>
  <div class="task-item-fondo">
    <div class="card">
      <div class="card2">
        <!-- los case hacen relacion a el boton de completar -->
        <h3 :class="props.task.is_complete ? 'done' : 'pending'">
          {{ task.title }}
        </h3>
        <p :class="props.task.is_complete ? 'done' : 'pending'">
          {{ task.description }}
        </p>
        <!-- <button @click=>Marcar como completada</button> -->
        <div class="buttons">
          <button @click="completedTask" class="botton1">
            <img
              src="../../assets/vecteezy_check-mark-icon-sign-symbol-design_10141449_805.png"
              alt="Completar"
            />
            <!-- {{ task.is_complete ? "No completada 😐" : "Completada!" }} -->
          </button>

          <!-- boton de borrar tareas -->
          <button @click="showModalToggle" class="botton3">
            <img
              src="../../assets/TaskItem_Botons/papelera_borrar.png"
              alt="Borrar"
            />
          </button>

          <!-- boton para editar tareas -->
          <button @click="inputToggle" class="botton2">
            <img src="../../assets/TaskItem_Botons/editar.png" alt="Edit" />
          </button>
          <div v-if="showInput">
            <div class="container-edit">
              <div class="textInputWrapper">
                <input
                  v-model="newTitle"
                  placeholder="Introduce un nuevo titulo"
                  type="text"
                  class="textInput"
                />
              </div>
              <!-- <input
                type="text"
                v-model="newTitle"
                placeholder="Introduce un nuevo titulo"
              /> -->
            <div class="textInputWrapper">
                <input
                  v-model="newDescription"
                  placeholder="Introduce un nuevo descripción"
                  type="text"
                  class="textInput"
                />
              </div>
            <!-- <div>
              <p>Escribe una descripción</p>
              <input
                type="text"
                v-model="newDescription"
                placeholder="Introduce una nueva descripción"
              />
            </div> -->
            <button @click="sendData" class="button8">
              <span class="button8-content">Actualizar tarea</span>
            </button>
            <!-- <button @click="sendData"><p>Enviar datos</p></button> -->
          </div>
        </div>
        </div>
      </div>
    </div>
  </div>
  <div class="aviso-modal" v-if="showModal">
    <div class="modal">
      <img
        class="warning"
        src="../../assets/warning-icon-png-2756.png"
        alt="alert"
      />

      <div class="columns-modal">
        <h2>
          Seguro que quieres <br />
          borrar esta tarea?
        </h2>
        <div class="buttons-modal">
          <button @click="deleteTask" class="button8">
            <span class="button8-content">Si, quiero borrar! </span>
          </button>
        </div>

        <button @click="showModalToggle" class="button8">
          <span class="button8-content">No, quiero cancelar </span>
        </button>
        <!-- <button @click="deleteTask">Si quiero!</button>
    <button @click="showModalToggle">No, mejor no!</button> -->
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task";
import { supabase } from "../supabase";

// funcion para refrescar al editar, borrar o completar
const emit = defineEmits(["childComplete", "editChild"]);
// const emit = defineEmits(["updateTask"]);

const taskStore = useTaskStore();

const props = defineProps({
  task: Object,
});

const showInput = ref(false);
const newTitle = ref("");
const newDescription = ref("");

function inputToggle() {
  showInput.value = !showInput.value;
}

//funcion para completar tareas
const completedTask = () => {
  emit("childComplete", props.task);
};

// Función para borrar la tarea a través de la store. El problema que tendremos aquí (y en NewTask.vue) es que cuando modifiquemos la base de datos los cambios no se verán reflejados en el v-for de Home.vue porque no estamos modificando la variable tasks guardada en Home. Usad el emit para cambiar esto y evitar ningún page refresh.
const deleteTask = async () => {
  await taskStore.deleteTask(props.task.id);
  emit("updateTask");
};

//funcion para mostrar el modal para el delete
const showModal = ref(false);
const showModalToggle = () => {
  showModal.value = !showModal.value;
};

const showErrorMess = ref(false);
const errorMess = ref(null);

//funcion para editar las tareas
const sendData = async () => {
  if (newTitle.value.length < 4 || newDescription.value.length < 4) {
    showErrorMess.value = true;
    errorMess.value =
      "El titulo o la descripción de la tarea son muy cortos! Escribe algo más!";
    setTimeout(() => {
      showErrorMess.value = false;
    }, 5000);
  } else {
    taskStore.editTask(newTitle.value, newDescription.value, props.task.id);
    showInput.value = !showInput.value;
    emit("editChild", newTaskEdited);
  }
};
</script>

<style>
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 9999;
  background: linear-gradient(145deg, #ffffff, #e6b3a5);
  width: 50vw;
  height: 50vh;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 5vw;
  color: white;
  animation: fade-in 0.5s forwards, modal-grow 0.5s forwards;
}

@keyframes modal-grow {
  from {
    transform: translate(-50%, -50%) scale(0.8);
  }
  to {
    transform: translate(-50%, -50%) scale(1);
  }
}

@keyframes fade-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.warning {
  width: 15vw;
}

.aviso-modal {
  z-index: 1;
  background-color: #00000080;
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
.button8 {
  position: relative;
  overflow: hidden;
  height: 3rem;
  padding: 0 2rem;
  border-radius: 1.5rem;
  background: #3d3a4e;
  background-size: 400%;
  color: #fff;
}

.button8:hover::before {
  transform: scaleX(1);
}

.button8-content {
  position: relative;
  z-index: 1;
}

.button8::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  transform: scaleX(0);
  transform-origin: 0 50%;
  width: 100%;
  height: inherit;
  border-radius: inherit;
  background: linear-gradient(
    82.3deg,
    rgb(233, 168, 93) 10.8%,
    rgb(228, 123, 52) 94.3%
  );
  transition: all 0.475s;
}

.buttons-modal {
  display: flex;
  gap: 2vw;
}

.columns-modal {
  display: flex;
  flex-direction: column;
  gap: 3vw;
}

.columns-modal h2 {
  color: #3d3a4e;
}

.textInputWrapper {
  position: relative;
  width: 180px;
  margin: 12px 5px;
  --accent-color: #fbf8f5;
}

.textInputWrapper:before {
  transition: border-bottom-color 200ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
  border-bottom: 1px solid rgba(247, 247, 247, 0.42);
}

.textInputWrapper:before,
.textInputWrapper:after {
  content: "";
  left: 0;
  right: 0;
  position: absolute;
  pointer-events: none;
  bottom: -1px;
  z-index: 4;
  width: 100%;
}

.textInputWrapper:focus-within:before {
  border-bottom: 3px solid var(--accent-color);
}

.textInputWrapper:before {
  transition: border-bottom-color 200ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
  border-bottom: 1px solid rgba(0, 0, 0, 0.42);
}

.textInputWrapper:focus-within:before {
  border-bottom: 1px solid var(--accent-color);
  transform: scaleX(1);
}

.textInputWrapper:focus-within:after {
  border-bottom: 2px solid var(--accent-color);
  transform: scaleX(1);
}

.textInputWrapper:after {
  content: "";
  transform: scaleX(0);
  transition: transform 250ms cubic-bezier(0, 0, 0.2, 1) 0ms;
  will-change: transform;
  border-bottom: 2px solid var(--accent-color);
  border-bottom-color: var(--accent-color);
}

.textInput::placeholder {
  transition: opacity 250ms cubic-bezier(0, 0, 0.2, 1) 0ms;
  opacity: 1;
  user-select: none;
  color: rgba(255, 255, 255, 0.582);
}

.textInputWrapper .textInput {
  border-radius: 5px 5px 0px 0px;
  box-shadow: 0px 2px 5px rgb(35 35 35 / 30%);
  max-height: 36px;
  background-color: #252525;
  transition-timing-function: cubic-bezier(0.25, 0.8, 0.25, 1);
  transition-duration: 200ms;
  transition-property: background-color;
  color: #e8e8e8;
  font-size: 14px;
  font-weight: 500;
  padding: 12px;
  width: 100%;
  border-left: none;
  border-bottom: none;
  border-right: none;
}

.textInputWrapper .textInput:focus,
.textInputWrapper .textInput:active {
  outline: none;
}

.textInputWrapper:focus-within .textInput,
.textInputWrapper .textInput:focus,
.textInputWrapper .textInput:active {
  background-color: #e48970;
}

.textInputWrapper:focus-within .textInput::placeholder {
  opacity: 0;
}

.container-edit {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.botton1:hover {
  background-image: url("../../assets/vecteezy_check-mark-icon-sign-symbol-design_10141449_805.png");
}
</style>

<!--
**Hints**
1. ref() or reactive() can be used here to store the following, think if you want to store them either individually or
like an object, up to you.

2. Data properties need here are the following: a boolean to store a false**Important variable, a string to store an error,
a string to store the value of the task that the user can edit, an initial false boolean to hide the inputFIeld used to edit
the new task detail or details[this is in reference of the task title and the task description].

3. Store the custom emit events that will be used to call the functions of the homeView for editing, deleting and toggling the
status[completed, not complted] of the taskItem.

4. Function to handle the error with the data properties used to control the error and the the boolean controlling the boolean
empty variable.

5. Function to handle the edit dialogue where the inputField is displayed and the string used to store the value of the
inputField will be used here to save the value as a prop on this function.

6. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the toggle completion of the task on the homeview.

7. Function to edit the task information that you decided that the user can edit. This function's body takes in a conditional
that first checks if the value of the task [either title and description or just title] is empty which if true it runs the
function used to handle the error on hint4. Else, the conditional sets the first mentioned boolean data property on hint2
back to its inital boolean value to hide the error message and repeat the same for the data property mentioned 4th on hint2;
a constant that stores an object that holds the oldValue from the prop item and the value of the task coming from the data
property mentioned 3rd on hint2; a custom event emit() that takes 2 parameters a name for the custom event and the value
from the object used on this part of the conditional and lastly this part of the conditional sets the value of input field
to an empty string to clear it from the ui.

8. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the removal of  the task on the homeview.

**********************************************************************
Se pueden usar "ref()" o "reactive()" para almacenar los siguientes elementos, dependiendo si se quieren almacenar individualmente o como un objeto.

Las propiedades de datos necesarias son las siguientes: una variable booleana para almacenar una variable "Importante" en falso, una cadena de texto para almacenar un error, una cadena de texto para almacenar el valor de la tarea que el usuario puede editar, y una variable booleana inicial para ocultar el campo de entrada utilizado para editar el título y descripción de la tarea.

Almacenar los eventos personalizados que se usarán para llamar a las funciones de la vista principal para editar, eliminar y alternar el estado de la tarea.

Función para manejar el error con las propiedades de datos mencionadas anteriormente para controlar el error y la variable booleana que controla la variable booleana vacía.

Función para manejar el diálogo de edición donde se muestra el campo de entrada y la cadena de texto utilizada para almacenar el valor del campo de entrada se usará aquí para guardar el valor como prop en esta función.

Función para emitir un evento personalizado que tome 2 parámetros: un nombre para el evento personalizado y el valor que se enviará a través de la prop al componente padre. Esta función puede controlar la alternación de la finalización de la tarea en la vista principal.

Función para editar la información de la tarea que se decidió que el usuario puede editar. El cuerpo de la función toma una condicional que primero verifica si el valor de la tarea (ya sea el título y la descripción o solo el título) está vacío, en cuyo caso se ejecuta la función utilizada para manejar el error en la sugerencia 4. De lo contrario, la condicional establece la primera variable booleana mencionada en la sugerencia 2 de nuevo en su valor booleano inicial para ocultar el mensaje de error y hace lo mismo con la propiedad de datos mencionada en la sugerencia 4; una constante que almacena un objeto que contiene el valor antiguo de la prop item y el valor de la tarea que proviene de la propiedad de datos mencionada en la sugerencia 3; un evento personalizado que toma 2 parámetros: un nombre para el evento personalizado y el valor del objeto utilizado en
***********************************************************************
-->
