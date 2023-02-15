<template>
  <nav class="menu">
    <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
    <img
      class="navbar-img"
      src="/assets/logowu.png"
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
          <router-link class="links-visitados" to="/"
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
  padding-top: 5vh;
  padding-bottom: 15vh;
  position: relative;
  z-index: 2;
  font-size: 1.3vw;
  font-weight: bold;
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
  width: 7vw;
  border-radius: 0%;
}

.links-visitados:visited {
  color: #fff;
  text-decoration: none;
}

.gradient-text {
  background-image: linear-gradient(to left, #efdfdf, #e48970);
  -webkit-background-clip: text; /* Prefijo necesario para que funcione en Chrome */
  background-clip: text;
  color: transparent;
}

.log-out ul {
  display: flex;
  flex-direction: row;
  gap: 3vw;
}

.log-out ul li p {
  font-size: 0.9vw;
}

.log-out ul li p span{
  /* color: #e48970; */
}
</style>
