<template>
  <NavTopDeconnexion />
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <hello-world msg="Welcome to Your Vue.js App" />
  </div>

  <div class="foot">
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
import NavTopDeconnexion from "../components/NavTopDeconnexion.vue";
import HelloWorld from "../components/HelloWorld.vue";
import Footer from "../components/Footer.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
    Footer,
    NavTopDeconnexion: NavTopDeconnexion,
  },
};
</script>
