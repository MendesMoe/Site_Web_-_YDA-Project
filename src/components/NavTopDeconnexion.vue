<template>
  <!--Component Navbarre pour ADMIN réutilisable de partout-->
  <div class="nav_top">
    <ul>
      <li>
        <router-link to="/" @click="clearLocalStorage">
          <div class="fav_icon">
            <p><i class="fas fa-power-off"></i></p>
          </div>
        </router-link>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  inject: ["role"],
  data() {
    return {};
  },
  methods: {
    clearLocalStorage() {
      localStorage.removeItem("@token");
      localStorage.removeItem("@id");
    },
  },
};
</script>
<style scoped>
.nav_top {
  background-color: #faf9f7;
  height: 5%;
  width: 100%;
  display: flex;
  margin-top: 0%;
  border: 1px solid rgb(22, 22, 22);
}
.fav_icon {
  color: rgb(10, 0, 0);
  text-align: right;
  position: absolute;
  right: 5%;
  font-size: 20px;
  margin-top: -9px;
}
.fav_icon p i {
  margin-left: 8px;
}
ul {
  display: flex;
  justify-content: flex-end;
  margin: 0;
}
li {
  list-style: none;
}
</style>