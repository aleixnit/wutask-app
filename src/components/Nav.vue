<template>
  <nav class="menu">
    <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
    <img
      class="navbar-img"
      src="/assets/165307706_10159118078709812_332331654461491189_n.jpg"
      alt=""
    />
    <router-link class="links-visitados" to="/"> Home </router-link>

    <!-- Esto es el menú central del navegador -->
    <div class="">
      <ul>
        <li>
          <!-- <button class="button-nav"> -->
          <router-link class="links-visitados" to="/">Reloj</router-link>
          <!-- </button> -->
        </li>
      </ul>
    </div>
    <div class="">
      <ul>
        <li>
          <router-link class="links-visitados" to="/account"
            >Perfil</router-link
          >
        </li>
      </ul>
    </div>

    <div class="log-out">
      <ul>
        <li>
          <p>Welcome back, {{ user.email }}</p>
        </li>
        <li>
          <button @click="signOut" class="botton1">Cerrar sesión</button>
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
/* .menu {
  height: 15vh;
  color: white;
  background-color: black;
}
.menu-central ul {
  display: flex;
  flex-direction: row;
  gap: 15vw;
  list-style: none;
  text-decoration: none;
  color: #fff;
  display: flex;
  width: 100%;
  justify-content: space-around;
  align-items: center;
} */

nav {
  background-color: transparent;
  display: flex;
  width: 100%;
  justify-content: space-around;
  align-items: center;
  padding-top: 10vh;
  padding-bottom: 20vh;
  position: relative;
  z-index: 2;
}

nav a {
  text-decoration: none;
}
nav ul {
  list-style: none;
  /* padding-inline-start: 0; */
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #fff;
}

.navbar-img {
  width: 5vw;
}

.links-visitados:visited {
  color: #fff;
  text-decoration: none;
}

.log-out ul {
  display: flex;
  flex-direction: row;
  gap: 3vw;
}
</style>
