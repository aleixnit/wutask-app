<template>
  <div class="container">
    <img src="/assets/logowu3.png" alt="Logo Task App" />
    <div class="header">
      <div class="header-description">
        <h3>¡Registrate a WuTask App!</h3>
        <p class="header-subtitle">
          ¡Es muy fácil y solo te llevará un minuto!
        </p>
      </div>
    </div>

    <form @submit.prevent="signUp" class="sign-up">
      <div class="form">
        <div class="form-input">
          <div class="inputSignin">
            <input
              placeholder="example@gmail.com"
              type="email"
              id="email"
              v-model="email"
              class="input"
              required
            />
            <span class="highlight"></span>
            <span class="bar"></span>
            <label for="email">Email</label>
          </div>
        </div>

        <div class="inputSignin">
          <input
            placeholder="********"
            type="password"
            id="password"
            v-model="password"
            class="input"
            required
          />
          <span class="highlight"></span>
          <span class="bar"></span>
          <label for="password">Contraseña</label>
        </div>

        <div class="inputSignin">
          <input
            placeholder="********"
            type="password"
            id="confirmPassword"
            v-model="confirmPassword"
            class="input"
            required
          />
          <span class="highlight"></span>
          <span class="bar"></span>
          <label for="password">Confirma tu contraseña</label>
        </div>

        <button type="submit" data-text="Awesome" class="button6">
          <span class="actual-text">&nbsp;Regístrate&nbsp;</span>
          <span class="hover-text" aria-hidden="true"
            >&nbsp;Regístrate&nbsp;</span
          >
        </button>
        <p>
          Ya tienes una cuenta?
          <PersonalRouter
            :route="route"
            :buttonText="buttonText"
            class="sign-up-link"
          />
        </p>
      </div>
    </form>

    <div v-show="errorMsg">{{ errorMsg }}</div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// Route Variables
const route = "/auth/login";
const buttonText = "Inicia sesión";

// Input Fields
const email = ref("");
const password = ref("");
const confirmPassword = ref("");

// Error Message
const errorMsg = ref("");

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to SignUp user to supaBase with a timeOut() method for showing the error
const signUp = async () => {
  if (password.value === confirmPassword.value) {
    try {
      // calls the user store and send the users info to backend to logIn
      await useUserStore().signUp(email.value, password.value);
      // redirects user to the homeView
      redirect.push({ path: "/auth/login" });
    } catch (error) {
      // displays error message
      errorMsg.value = error.message;
      // hides error message
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    }
    return;
  }
  errorMsg.value = "¡Las credenciales són inválidas o ya estás registrado!";
};
</script>

<style>
.container {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-image: linear-gradient(rgba(58, 24, 3, 0.7), rgba(0, 0, 0, 0.9)),
    url("../../assets/fondotaskitem2.png");
  background-size: cover;
  color: white;
  animation: fondoAnimadoNewTask2 40s ease 0s infinite alternate forwards;
}

.container h3 {
  font-size: 3vw;
  margin-bottom: 2vw;
  color: #e8b3a5;
}

.button-signup {
  margin-bottom: 3vw;
  margin-top: 2vw;
}

.container img {
  border-radius: 0px;
  width: 8vw;
  display: flex;
  justify-content: center;
  align-items: center;
}

.form p {
  margin-top: 2vw;
}


</style>
