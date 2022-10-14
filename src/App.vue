<template>
  <div class="container" v-if="$route.name !== 'Connexion'">
    <div class="menu-left">
      <MenuLeft />
    </div>
    <div class="main">
      <NavTopDeconnexion />
      <router-view />
    </div>
  </div>
  <div class="container" v-else>
    <router-view />
  </div>
</template>

<script>
import { computed } from "vue";
import NavTopDeconnexion from "./components/NavTopDeconnexion.vue";
import MenuLeft from "../src/components/MenuLeft.vue";

export default {
  components: {
    NavTopDeconnexion,
    MenuLeft,
  },
  data() {
    return {
      role: "",
      token: localStorage.getItem("@token"),
    };
  },

  provide() {
    return {
      role: computed(() => this.role),
      setRole: (value) => {
        this.role = value;
      },
    };
  },
  async mounted() {
    const url = "http://127.0.0.1:8000/api/user";

    const options = {
      method: "GET",
      headers: {
        Authorization: "Bearer " + localStorage.getItem("@token"),
        "Content-Type": "application/json",
        Accept: "application/json",
      },
    };

    const response = await fetch(url, options);

    if (response.status !== 200) {
      localStorage.removeItem("@token");
      this.$router.replace("/connexion");
    } else {
      const data = await response.json();
      this.role = data.role;
    }
  },
};
</script>

<style>
#app {
  font-family: Trebuchet MS, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  width: 100vw;
}

.container {
  font-family: "Trebuchet MS";
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 100%;
  margin-top: -10px;
  margin-left: -10px;
}
.menu-left {
  width: 18%;
  height: 100%;
}
.main {
  width: 100%;
  height: 100%;
  background: rgb(244, 240, 240);
  display: flex;
  flex-direction: column;
  padding-top: 0%;
}
</style>
