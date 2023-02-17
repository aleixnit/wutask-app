<template>
  <div class="fondoperfil">
    <Nav />
    <h1>Name: {{ username }}</h1>
    <img
      :src="
        avatar_url
          ? avatar_url
          : 'https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460__480.png'
      "
      alt="Profile picture"
    />
    <div>
      <h2>Email: {{ username }}</h2>
      <h2>Website: {{ website }}</h2>
    </div>
    <button @click.prevent="editProfileButton">Edit your profile</button>

    <form class="form-widget" @submit.prevent="updateProfile">
    <!-- Add to body -->
    <Avatar v-model:path="avatar_url" @upload="updateProfile" size="10" />

    <!-- Other form elements -->
  </form>
    <FooterComp />
  </div>
</template>

<script setup>
import { supabase } from "../supabase";
import { onMounted, ref, toRefs } from "vue";
import { useUserStore } from "../stores/user";
import Nav from "../components/Nav.vue";
import Avatar from '../components/Avatar.vue';
import FooterComp from "../components/FooterNew.vue";
import { useRouter } from "vue-router";


const redirect = useRouter();

const userStore = useUserStore();

const loading = ref(false);
const username = ref(null);
const website = ref(null);
const avatar_url = ref(null);
const name = ref(null);

onMounted(() => {
  getProfile();
});

async function getProfile() {
  await userStore.fetchUser();
  username.value = userStore.profile.username;
  avatar_url.value = userStore.profile.avatar_url;
  website.value = userStore.profile.website;
  name.value = userStore.profile.name;
}

async function signOut() {
  try {
    loading.value = true;
    let { error } = await supabase.auth.signOut();
    if (error) throw error;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
}

const editProfileButton = () => {
  redirect.push({ path: "/editprofile" });
};
</script>

<style></style>
