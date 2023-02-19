<!-- COMPONENTE BOILERPLATE -->

<template>
  <div class="fondo">
    <div class="loginpage">
      <img src="/assets/logowu3.png" alt="Logo Task App" />
      <h3>¡Bienvenido al WuTask App!</h3>
      <p class="refactClass">¡Mantén las tareas de tu dia a dia organizadas!</p>


      <!-- FORMULARIO -->
      <!-- <form @submit.prevent="signIn"> -->
      <!-- <div class="form">
          <div class="form-input">
            <label for="email" class="input-field-label" />
            <input
              type="email"
              class="input-field"
              id="email"
              placeholder="Introduce tu correo"
              required
              v-model="email"
            />
          </div>
          <div class="form-input">
            <label for="password" class="input-field-label" />
            <input
              type="password"
              id="password"
              class="input-field"
              placeholder="Introduce tu contraseña"
              v-model="password"
              required
            />
          </div> -->
      <!-- inputs nuevos!!!!!!!!!!! -->
      <form @submit.prevent="signIn">
        <div class="inputSignin">
          <input required="" type="email" v-model="email" class="input" />
          <span class="highlight"></span>
          <span class="bar"></span>
          <label for="email">Email</label>
        </div>

        <div class="inputSignin">
          <input required="" type="password" v-model="password" class="input" />
          <span class="highlight"></span>
          <span class="bar"></span>
          <label for="password">Contraseña</label>
        </div>
        <div class="iniciarRegistro">
          <!-- REGISTRARSE / SIGN UP -->
          <button class="fancy" type="submit">
            <!-- boton animado -->
            <span class="top-key"></span>
            <span class="text">Iniciar Sesión</span>
            <span class="bottom-key-1"></span>
            <span class="bottom-key-2"></span>
          </button>

          <p>
            No tienes una cuenta?
            <PersonalRouter
              :route="route"
              :buttonText="buttonText"
              class="sign-up-link"
            />
          </p>
        </div>
      </form>
    </div>
  </div>
  <!-- </div> -->
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// Route Variables
const route = "/auth/signup";
const buttonText = "¡Registrate!";

// Input Fields
const email = ref("");
const password = ref("");

// Error Message
const errorMsg = ref("");

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Login Function
const signIn = async () => {
  try {
    await useUserStore().signIn(email.value, password.value);
    redirect.push({ path: "/" });
  } catch (error) {
    errorMsg.value = `Error: ${error.message}`;
    setTimeout(() => {
      errorMsg.value = null;
    }, 2500);
  }
};
</script>

<style scoped></style>
