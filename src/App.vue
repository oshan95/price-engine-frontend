<template>
  <Header />
  <router-view></router-view>
</template>

<script>
import Header from "./components/Header";
import { createRouter, createWebHistory } from "vue-router";

export default {
  name: "App",
  components: {
    Header,
  },
};
</script>
