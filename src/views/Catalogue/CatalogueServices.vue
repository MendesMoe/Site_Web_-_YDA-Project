<template>
  <!--page d'afffichage du catalogue des services avec imports de différent composants-->

  <Navbarre v-if="this.role.value !== 'admin'" />
  <NavTopDeconnexion v-if="this.role.value == 'admin'" />
  <AllServices />
</template>

<script>
import NavTopDeconnexion from "../../components/NavTopDeconnexion.vue";
import AllServices from "../../components/Catalogue/AllServices.vue";
import Navbarre from "../../components/Navbarre.vue";

export default {
  inject: ["role"],
  data() {
    return {
      globalCart: [],
    };
  },
  components: {
    NavTopDeconnexion,
    AllServices,
    Navbarre,
  },
  methods: {},
};
</script>

