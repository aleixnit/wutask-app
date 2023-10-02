<!-- COMPONENTE BOILERPLATE -->

<template>
  <div id="body" class="hiddenloader">
    <div class="centrado" id="onload">
      <div class="spinner"></div></div>

    <div class="signin_fondo">
      <div class="signin_content">
        <img src="/assets/wulogo.png" alt="Logo Task App" />
        <h3>
          ¡Bienvenido <br />al
          <span style="color: rgb(255, 230, 0)">WuTask</span> App!
        </h3>
        <p>
          With the ability to<span class="bold">
            register, create, edit, complete, and delete tasks,</span
          >
          this app helps you stay<span class="bold">on top</span>
          of your to-do list with a touch of
          <span class="bold"> Wu-Tang flair. </span>
        </p>
        <!-- inputs nuevos!!!!!!!!!!! -->
        <form class="signin_form_box" @submit.prevent="signIn">
          <!--INput new sept-->
          <div class="signin_form">
            <input
              required=""
              type="email"
              v-model="email"
              class="signin_input"
            />
            <label for="email">
              <span style="transition-delay: 0ms; left: 0px; top: -25px"
                >E</span
              >
              <span style="transition-delay: 75ms; left: 20px; top: -25px"
                >m</span
              >
              <span style="transition-delay: 150ms; left: 47px; top: -25px"
                >a</span
              >
              <span style="transition-delay: 225ms; left: 64px; top: -25px"
                >i</span
              >
              <span style="transition-delay: 300ms; left: 75px; top: -25px"
                >l</span
              >
              <p
                style="
                  position: absolute;
                  left: -8px;
                  top: -15px;
                  font-size: 17px;
                  margin: 10px;
                  color: gray;
                  transition: 0.5s;
                  pointer-events: none;
                "
              >
                Email
              </p>
            </label>
          </div>
          <!--INput new sept-->
          <div class="signin_form">
            <input
              required=""
              type="password"
              v-model="password"
              class="signin_input"
            />
            <label for="password">
              <span style="transition-delay: 0ms; left: 0px; top: -25px"
                >P</span
              >
              <span style="transition-delay: 75ms; left: 20px; top: -25px"
                >a</span
              >
              <span style="transition-delay: 150ms; left: 33px; top: -25px"
                >s</span
              >
              <span style="transition-delay: 225ms; left: 47px; top: -25px"
                >s</span
              >
              <span style="transition-delay: 300ms; left: 56px; top: -25px"
                >w</span
              >
              <span style="transition-delay: 375ms; left: 72px; top: -25px"
                >o</span
              >
              <span style="transition-delay: 450ms; left: 85px; top: -25px"
                >r</span
              >
              <span style="transition-delay: 525ms; left: 95px; top: -25px"
                >d</span
              >
              <p
                style="
                  position: absolute;
                  left: -8px;
                  top: -15px;
                  font-size: 17px;
                  margin: 10px;
                  color: gray;
                  transition: 0.5s;
                  pointer-events: none;
                "
              >
                Password
              </p>
            </label>
          </div>
          <div class="iniciarRegistro">
            <!-- REGISTRARSE / SIGN UP -->
            <button type="submit" data-text="Awesome" class="signin_button">
              <span class="signin_button_span">INICIAR</span>
              <span class="signin_button_second">
                <svg
                  width="50px"
                  height="20px"
                  viewBox="0 0 66 43"
                  version="1.1"
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                >
                  <g
                    id="arrow"
                    stroke="none"
                    stroke-width="1"
                    fill="none"
                    fill-rule="evenodd"
                  >
                    <path
                      class="signin_button_one"
                      d="M40.1543933,3.89485454 L43.9763149,0.139296592 C44.1708311,-0.0518420739 44.4826329,-0.0518571125 44.6771675,0.139262789 L65.6916134,20.7848311 C66.0855801,21.1718824 66.0911863,21.8050225 65.704135,22.1989893 C65.7000188,22.2031791 65.6958657,22.2073326 65.6916762,22.2114492 L44.677098,42.8607841 C44.4825957,43.0519059 44.1708242,43.0519358 43.9762853,42.8608513 L40.1545186,39.1069479 C39.9575152,38.9134427 39.9546793,38.5968729 40.1481845,38.3998695 C40.1502893,38.3977268 40.1524132,38.395603 40.1545562,38.3934985 L56.9937789,21.8567812 C57.1908028,21.6632968 57.193672,21.3467273 57.0001876,21.1497035 C56.9980647,21.1475418 56.9959223,21.1453995 56.9937605,21.1432767 L40.1545208,4.60825197 C39.9574869,4.41477773 39.9546013,4.09820839 40.1480756,3.90117456 C40.1501626,3.89904911 40.1522686,3.89694235 40.1543933,3.89485454 Z"
                      fill="#FFFFFF"
                    ></path>
                    <path
                      class="signin_button_two"
                      d="M20.1543933,3.89485454 L23.9763149,0.139296592 C24.1708311,-0.0518420739 24.4826329,-0.0518571125 24.6771675,0.139262789 L45.6916134,20.7848311 C46.0855801,21.1718824 46.0911863,21.8050225 45.704135,22.1989893 C45.7000188,22.2031791 45.6958657,22.2073326 45.6916762,22.2114492 L24.677098,42.8607841 C24.4825957,43.0519059 24.1708242,43.0519358 23.9762853,42.8608513 L20.1545186,39.1069479 C19.9575152,38.9134427 19.9546793,38.5968729 20.1481845,38.3998695 C20.1502893,38.3977268 20.1524132,38.395603 20.1545562,38.3934985 L36.9937789,21.8567812 C37.1908028,21.6632968 37.193672,21.3467273 37.0001876,21.1497035 C36.9980647,21.1475418 36.9959223,21.1453995 36.9937605,21.1432767 L20.1545208,4.60825197 C19.9574869,4.41477773 19.9546013,4.09820839 20.1480756,3.90117456 C20.1501626,3.89904911 20.1522686,3.89694235 20.1543933,3.89485454 Z"
                      fill="#FFFFFF"
                    ></path>
                    <path
                      class="signin_button_three"
                      d="M0.154393339,3.89485454 L3.97631488,0.139296592 C4.17083111,-0.0518420739 4.48263286,-0.0518571125 4.67716753,0.139262789 L25.6916134,20.7848311 C26.0855801,21.1718824 26.0911863,21.8050225 25.704135,22.1989893 C25.7000188,22.2031791 25.6958657,22.2073326 25.6916762,22.2114492 L4.67709797,42.8607841 C4.48259567,43.0519059 4.17082418,43.0519358 3.97628526,42.8608513 L0.154518591,39.1069479 C-0.0424848215,38.9134427 -0.0453206733,38.5968729 0.148184538,38.3998695 C0.150289256,38.3977268 0.152413239,38.395603 0.154556228,38.3934985 L16.9937789,21.8567812 C17.1908028,21.6632968 17.193672,21.3467273 17.0001876,21.1497035 C16.9980647,21.1475418 16.9959223,21.1453995 16.9937605,21.1432767 L0.15452076,4.60825197 C-0.0425130651,4.41477773 -0.0453986756,4.09820839 0.148075568,3.90117456 C0.150162624,3.89904911 0.152268631,3.89694235 0.154393339,3.89485454 Z"
                      fill="#FFFFFF"
                    ></path>
                  </g>
                </svg>
              </span>
            </button>
            <p>
              No tienes una cuenta?
              <PersonalRouter
                :route="route"
                :buttonText="buttonText"
                class="signin_button_register"
              />
            </p>
          </div>
        </form>
      </div>
      <div class="signin_box2">
        <div class="video-background">
          <video autoplay muted loop class="video-bg">
            <source src="../../assets/wutaskvideo2.mp4" type="video/mp4" />
            Tu navegador no soporta el elemento de video.
          </video>
          <!-- Contenido de tu componente aquí -->
        </div>
      </div>
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

//PRELOADER PAGINA WEB
window.onload = function () {
  setTimeout(function () {
    $("#onload").css("opacity", "0"); // Cambia la opacidad a 0 para que se desvanezca
    setTimeout(function () {
      $("#onload").hide(); // Oculta el elemento después de la transición
    }, 500); // Espera 0.5 segundos antes de ocultar el elemento
    $("#body").removeClass("hiddenloader");
  }, 1000); // 1500 milisegundos = 1.5 segundos
};
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
