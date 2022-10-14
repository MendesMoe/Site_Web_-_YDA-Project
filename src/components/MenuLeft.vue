<template>
  <div class="menu-content">
    <div class="logo">
      <img
        v-if="this.role.value == 'admin'"
        src="../assets/img/logo_without_title.png"
        style="width: 115px"
      />
      <img v-else src="../assets/img/images.png" style="width: 115px" />
    </div>
    <div class="menu-list">
      <p v-if="this.role.value == 'admin'">
        <router-link class="anim" to="/dashboard">Home</router-link>
      </p>
      <p v-if="this.role.value == 'member'">
        <router-link class="anim" to="/dashMember">Home</router-link>
      </p>
      <p v-if="this.role.value == 'admin'">
        <router-link class="anim" to="/AllFirms">Entreprises</router-link>
      </p>
      <p>
        <router-link class="anim" to="/catalogue/services"
          >Catalogue</router-link
        >
      </p>
      <p v-if="this.role.value == 'admin'">
        <router-link class="anim" to="">Agenda</router-link>
      </p>
      <p v-if="this.role.value == 'admin'">
        <router-link class="anim" to="/creation">
          Gestion des comptes</router-link
        >
      </p>
      <p v-if="this.role.value == 'member'">
        <router-link class="anim" to="/DashMember">Mes commandes</router-link>
      </p>

      <p v-if="this.role.value == 'member'">
        <router-link class="anim" to="/EditProfilMember"
          >Mon profil</router-link
        >
      </p>
      <p v-if="this.role.value == 'member'">
        <router-link class="anim" to="/panier">
          <i class="fas fa-shopping-cart"> Panier </i>
          <div v-if="notif != null" class="notif">1</div>
        </router-link>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  inject: ["role"],
};
</script>

<style scoped>
.logo img {
  margin-top: 9%;
}
.menu-content {
  display: flex;
  flex-direction: column;
  height: 100%;
  background-color: rgb(6, 6, 6);
  align-items: center;
  justify-content: space-between;
}
.menu-list {
  display: flex;
  flex-direction: column;
  margin: 60% 0%;
  height: 100%;
  align-content: center;
  justify-content: space-between;
}
.anim {
  content: "";
  text-decoration: none;
  color: #ffffff;
  margin: 3px 8px;
}
.anim::before {
  padding: 5px 0;
  margin: 0 8px;
  content: "";
  border-left: 2px solid transparent;
  color: #ffffff;
}
.anim::after {
  padding: 5px 0;
  margin: 0 8px;
  content: "";
  border-right: 2px solid transparent;
  color: #ffffff;
}
.anim:hover::before {
  margin: 0 8px;
  content: "";
  animation: btn1 0.5s;
  animation-fill-mode: forwards;
}
.anim:hover::after {
  margin: 0 8px;
  content: "";
  animation: btn1 0.5s;
  animation-fill-mode: forwards;
}
@keyframes btn1 {
  from {
    margin: 0 8px;
    padding-top: 0px;
    background-color: #ffffff;
    opacity: 1;
  }
  to {
    margin: 0 8px;
    padding-top: 8px;
    background-color: #ffffff;
    opacity: 1;
  }
}
</style>