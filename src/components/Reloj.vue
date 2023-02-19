<template>
    <div class="bodyclock">
    <div id="clock">
    <p class="date">{{ date }}</p>
    <p class="time">{{ time }}</p>
    <p class="text">DIGITAL CLOCK with Vue.js</p>
</div>
</div>
</template>

<script setup>
import { useRouter } from "vue-router";
import { ref } from "vue";
const redirect = useRouter();
</script>

<style>
.bodyclock {
    height: 60vh;
    background-color: black;
}
</style>