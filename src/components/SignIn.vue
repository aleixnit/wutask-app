<!-- COMPONENTE BOILERPLATE -->

<template>
  <div class="fondo">
    <div class="loginpage">
      <img src="/assets/Logo1.png" alt="Logo Task App" />
      <h3>Bienvenido a la App de tus Tareas</h3>
      <p>Organiza las tareas de tu dia a dia</p>

      <!-- FORMULARIO -->
      <form @submit.prevent="signIn">
        <div class="form">
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
          </div>

          <!-- REGISTRARSE / SIGN UP -->
          <button type="submit" class="botton1">
            Iniciar sesión

            <!-- boton animado -->
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
const buttonText = "Sign Up";

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

<style scoped>
.fondo {
  background-image: url("../../assets/fondo-banner-computacion-nube-ciudad-inteligente.jpg");
  background-size: cover;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.loginpage {
  background: rgba(255, 255, 255, 0.5);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 90vh;
  width: 60vw;
}

.loginpage h3 {
  font-size: 2vw;
  margin-top: 2.5vw;
  margin-bottom: 1.5vw;
}

.loginpage p {
  padding-left: 20vw;
  padding-right: 20vw;
  margin-bottom: 1.5vw;
}

.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* animacion inputs */
.input-field {
  margin-top: 1.5vw;
  background-color: white;
  width: 40vw;
  height: 2vw;
  padding: 10px;
  border: none;
  border-radius: 5px;
}
/* .input-field:focus {
  color:  #4469af;
  background-color: white;
  outline-color:  #4469af;
  box-shadow: -3px -3px 15px #4469af;
  transition: .1s;
  transition-property: box-shadow;
} */
</style>
