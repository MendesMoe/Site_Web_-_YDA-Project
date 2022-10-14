<template>
  <AllFirms />
</template>

<script>
import AllFirms from "../components/AllFirms.vue";

export default {
  props: {
    firmId: String,
  },
  data() {
    return {
      token: localStorage.getItem("@token"),
    };
  },
  components: {
    AllFirms: AllFirms,
  },
};
</script>