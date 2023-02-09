<template>
  <nav class="menu">
    <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
    <img class="navbar-img"  src="/assets/Logo1.png" alt="">
    <router-link to="/"> Home </router-link>

    <!-- Esto es el menú central del navegador -->
    <div class="menu-central">
      <ul>
        <li>
          <router-link to="/">Reloj</router-link>
        </li>

        <li>
          <router-link to="/account">Perfil</router-link>
        </li>
      </ul>
    </div>

    <div>
      <ul>
        <li class="log-out-welcome">
          <p>Welcome back, {{ user.email }}</p>
        </li>
        <li>
          <button @click="signOut" class="button">Cerrar sesión</button>
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
  border: solid 1px red;
}

.navbar-img {
  width: 5vw;
}

.menu-central ul {
  display: flex;
  flex-direction: row;
  gap: 15vw;
  border: solid 1px red;
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
}
</style>
