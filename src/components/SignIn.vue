<!-- COMPONENTE BOILERPLATE -->

<template>
  <div class="fondo">
    <div class="loginpage">
      <img src="/assets/logowu3.png" alt="Logo Task App" />
      <h3>¡Bienvenido al WuTask App!</h3>
      <p class="refactClass">¡Mantén las tareas de tu dia a dia organizadas!</p>

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

          <button type="submit" data-text="Awesome" class="button6">
          <span class="actual-text">&nbsp;Iniciar&nbsp;</span>
          <span class="hover-text" aria-hidden="true"
            >&nbsp;Iniciar&nbsp;</span
          >
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
