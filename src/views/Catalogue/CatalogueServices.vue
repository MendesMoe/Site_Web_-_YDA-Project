<template>
  <!--page d'afffichage du catalogue des services avec imports de différent composants-->
  <AllServices />
</template>

<script>
import AllServices from "../../components/Catalogue/AllServices.vue";

export default {
  inject: ["role"],
  data() {
    return {
      globalCart: [],
    };
  },
  components: {
    AllServices,
  },
  methods: {},
};
</script>

