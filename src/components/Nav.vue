<template>
  <nav class="menu">
    <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
    <img
      class="navbar-img"
      src="/assets/logowu3.png"
      alt=""
    />
    <router-link class="links-visitados" to="/"
      ><a class="fancy" href="#">
        <span class="top-key"></span>
        <span class="text">Inicio</span>
        <span class="bottom-key-1"></span>
        <span class="bottom-key-2"></span>
      </a>
    </router-link>

    <!-- Esto es el menú central del navegador -->
    <div class="">
      <ul>
        <li>
          <!-- <button class="button-nav"> -->
          <router-link class="links-visitados" to="/reloj"
            ><a class="fancy" href="#">
              <span class="top-key"></span>
              <span class="text">Reloj</span>
              <span class="bottom-key-1"></span>
              <span class="bottom-key-2"></span> </a
          ></router-link>
          <!-- </button> -->
        </li>
      </ul>
    </div>
    <div class="">
      <ul>
        <li>
          <router-link class="links-visitados" to="/account"
            ><a class="fancy" href="#">
              <span class="top-key"></span>
              <span class="text">Perfil</span>
              <span class="bottom-key-1"></span>
              <span class="bottom-key-2"></span> </a
          ></router-link>
        </li>
      </ul>
    </div>

    <div class="log-out">
      <ul>
        <li>
          <p>Welcome back, <span class="gradient-text">{{ user.email }}</span></p>
        </li>
        <li>
          <a @click="signOut" class="fancy" href="#">
            <span class="top-key"></span>
            <span class="text">Cerrar sesión</span>
            <span class="bottom-key-1"></span>
            <span class="bottom-key-2"></span>
          </a>
          <!-- <button @click="signOut" class="botton1">Cerrar sesión</button> -->
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup>
// import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref } from "vue";
import { supabase } from "../supabase";

const user = supabase.auth.user();
//constant to save a variable that will hold the use router method
const route = "/";
const buttonText = "Todo app";

// constant to save a variable that will get the user from store with a computed function imported from vue
// const getUser = computed(() => useUserStore().user);
const getUser = useUserStore().user;

// constant that calls user email from the useUSerStore
const userEmail = getUser.email;

// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();

const signOut = async () => {
  try {
    // call the user store and send the users info to backend to signOut
    // then redirect user to the homeView BARRA DE ABAJO ES LA BUENA INICIAL
    // await useUserStore().signOut();
    await supabase.auth.signOut();
    redirect.push({ path: "/auth/login" });
  } catch (error) {}
};
</script>

<style>
</style>
