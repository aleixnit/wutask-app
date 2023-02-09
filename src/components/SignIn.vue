<!-- COMPONENTE BOILERPLATE -->
 
  <template>
  <div class="fondo">
    <div class="container">
      <img src="/assets/Logo1.png" alt="Logo Task App" />
      <h3>Bienvenido a la App de tus Tareas</h3>
      <p>Organiza las tareas de tu dia a dia</p>

      <!-- FORMULARIO -->
      <form class="form-sign-in" @submit.prevent="signIn">
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
          <button type="submit" class="registrarse">Iniciar sesión</button>
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

// // Arrow function to SignUp user to supaBase with a timeOut() method for showing the error
// const signIn = async () => {
//     try {
//       // calls the user store and send the users info to backend to logIn
//       const user = await useUserStore().signIn(email.value, password.value);
//       // redirects user to the homeView
//       redirect.push({ path: "/" });
//     } catch (error) {
//       // displays error message
//       errorMsg.value = error.message;
//       // hides error message
//       setTimeout(() => {
//         errorMsg.value = null;
//       }, 5000);
//   }
//   errorMsg.value = "error";
// };
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

.container {
  background: rgba(255, 255, 255, 0.5);
  border-radius: 15%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 95vh;
}

.container h3 {
  font-size: 2vw;
  margin-top: 2.5vw;
  margin-bottom: 1.5vw;
}

.container p {
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

.form-input input {
  width: 40vw;
  height: 3vw;
  margin-top: 1.5vw;
  margin-bottom: 1.5vw;
  border-radius: 0.7vw;
  border: 0vw;
  padding-left:3vw;
  padding-top: 1vw;
  padding-bottom: 1vw;
}

.registrarse {
  background-color: black;
  color: white;
  margin-top: 1.5vw;
  margin-bottom: 1.5vw;
  width: 40vw;
  height: 3vw;
  border-radius: 0.7vw;
  border: 0vw;
}

.registrarse:hover {
  background-color: rgb(68,105,175);
}
</style>
