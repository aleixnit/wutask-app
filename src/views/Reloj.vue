<script setup>

import { onMounted, ref, toRefs } from "vue";
import Nav from "../components/Nav.vue";
import FooterComp from "../components/FooterNew.vue";
import { useRouter } from "vue-router";

</script>

<template>
    <Nav />
    <FooterComp />
</template>


<style>
.bodyclock {
    background-color: black;
}
</style>