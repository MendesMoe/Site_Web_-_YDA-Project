<template>
  <!--page d'afffichage du catalogue des produits avec imports de différent composants-->
  <ProductsByServiceId :servicesId="this.servicesId" />
</template>

<script>
import ProductsByServiceId from "../../components/Catalogue/ProductsByServiceId.vue";

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
    ProductsByServiceId,
  },
  methods: {
    async mounted() {
      console.log(this.servicesId);
    },
  },
};
</script>