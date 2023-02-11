<template>
  <nav class="menu">
    <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
    <img class="navbar-img" src="/assets/165307706_10159118078709812_332331654461491189_n.jpg" alt="" />
    <router-link to="/"> Home </router-link>

    <!-- Esto es el menú central del navegador -->
    <div class="menu-central">
      <ul>
        <li>
          <!-- <button class="button-nav"> -->
          <router-link to="/">Reloj</router-link>
          <!-- </button> -->
        </li>
      </ul>
    </div>
    <div class="menu-central">
      <ul>
        <li>
          <router-link to="/account">Perfil</router-link>
        </li>
      </ul>
    </div>

    <div class="log-out menu-central">
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
.menu {
  height: 15vh;
  color: white;
  background-color: black;
}

.navbar-img {
  width: 5vw;
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
}

nav {
  background-color: lightgray;
  display: flex;
  width: 100%;
  justify-content: space-around;
  align-items: center;
}

nav ul {
  list-style: none;
  padding-inline-start: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #fff;
}

.menu-central {
  align-items: center;
  background-color: transparent;
  color: #fff;
  cursor: pointer;
  display: flex;
  font-family: ui-sans-serif, system-ui, -apple-system, system-ui, "Segoe UI",
    Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif,
    "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
  font-size: 1rem;
  font-weight: 700;
  line-height: 1.5;
  text-decoration: none;
  text-transform: uppercase;
  outline: 0;
  border: 0;
  padding: 1rem;
  list-style: none;
}

.menu-central:before {
  background-color: #fff;
  content: "";
  display: inline-block;
  height: 1px;
  margin-right: 10px;
  transition: all 0.42s cubic-bezier(0.25, 0.8, 0.25, 1);
  width: 0;
}

.menu-central:hover:before {
  background-color: #fff;
  width: 3rem;
  color: white;
}

.menu-central:visited {
  color: white;
  text-decoration: none;
}

.log-out {
  font-size: 1vw;
  gap: 0px;
}
</style>
