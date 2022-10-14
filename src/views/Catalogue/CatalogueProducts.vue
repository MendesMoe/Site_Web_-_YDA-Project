<template>
  <!--page d'afffichage du catalogue des produits avec imports de différent composants-->
  <Navbarre v-if="this.role.value !== 'admin'" />
  <NavTopDeconnexion v-if="this.role.value == 'admin'" />
  <ProductsByServiceId :servicesId="this.servicesId" />
</template>

<script>
import NavTopDeconnexion from "../../components/NavTopDeconnexion.vue";
import ProductsByServiceId from "../../components/Catalogue/ProductsByServiceId.vue";
import Navbarre from "../../components/Navbarre.vue";

export default {
  inject: ["role"],
  data() {
    return {
      globalCart: [],
    };
  },
  props: {
    servicesId: String,
  },

  components: {
    NavTopDeconnexion,
    ProductsByServiceId,
    Navbarre,
  },
  methods: {
    async mounted() {
      console.log(this.servicesId);
    },
  },
};
</script>