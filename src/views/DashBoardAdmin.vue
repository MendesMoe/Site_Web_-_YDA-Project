<template>
  <AllOrders></AllOrders>
</template>

<script>
import AllOrders from "../components/Orders/AllOrders.vue";

export default {
  data() {
    return {
      token: localStorage.getItem("@token"),
    };
  },
  components: {
    AllOrders,
  },
};
</script>
<style scoped>
.content-page {
  display: flex;
  flex-direction: row;
  height: 100%;
}
</style>
