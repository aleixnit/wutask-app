<template>
  
  <div class="card">
    <div class="card2">
      <h3 :class="props.task.is_complete ? 'case1': 'case2'" >{{ task.title }}</h3>
      <p :class="props.task.is_complete ? 'case1': 'case2'">{{ task.description }}</p>
      <!-- <button @click=>Marcar como completada</button> -->
      <div class="buttons">
        <button @click="completeTask" class="botton1">
          {{ task.is_complete ? "Sin Realizar 😐" : "Realizada!" }}
        </button>

        <!-- boton de borrar tareas -->
        <button @click="deleteTask" class="botton1"><p>Borrar {{ task.title }}</p> </button>

        <!-- boton para editar tareas -->
        <button @click="inputToggle" class="botton1"><p>Editar {{ task.title }}</p></button>
        <div v-if="showInput">
          <div>
            <p>Escribe un título</p>
            <input
              type="text"
              v-model="newTitle"
              placeholder="Insert title..."
            />
          </div>
          <div>
            <p>Escribe una descripción</p>
            <input
              type="text"
              v-model="newDescription"
              placeholder="Insert description..."
            />
          </div>
          <button @click="sendData"><p>Enviar datos</p></button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task";
import { supabase } from "../supabase";

const taskStore = useTaskStore();
const emit = defineEmits(["updateTask"]);

const props = defineProps({
  task: Object,
});

const showInput = ref(false);
const newTitle = ref("");
const newDescription = ref("");

function inputToggle() {
  showInput.value = !showInput.value;
}

// Función para borrar la tarea a través de la store. El problema que tendremos aquí (y en NewTask.vue) es que cuando modifiquemos la base de datos los cambios no se verán reflejados en el v-for de Home.vue porque no estamos modificando la variable tasks guardada en Home. Usad el emit para cambiar esto y evitar ningún page refresh.
const deleteTask = async () => {
  await taskStore.deleteTask(props.task.id);
  emit("updateTask");
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
    emit("updateTask");
    showInput.value = !showInput.value;
  }
};

//funcion para completar las tareas

const completeTask = () => {
    emit("childComplete", props.task)
}
const completedTask = ref(false)




</script>

<style>
.case2{
    text-decoration: none;
}
.case1{
    text-decoration: line-through;
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
