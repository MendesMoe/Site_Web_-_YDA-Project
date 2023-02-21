<template>
  <div class="form_connexion">
    <!--FORM -->
    <form @submit.prevent="connectUser()">
      <div class="items_form">
        <div class="img_container">
          <img src="../assets/img/logo_color.png" alt="" />
        </div>
        <!--EMAIL -->
        <div v-if="this.status == 500">
          <p class="p_red">E-mail ou mot de passe invalide</p>
        </div>
        <label for="username">Login</label>
        <input id="username" type="email" v-model="email" />
        <!--PASSWORD -->
        <label for="password">Password</label>
        <input id="password" type="password" v-model="password" />
        <router-link to="/connexion" @click="SendMagicLink"
          >Mot de passe oublié ?</router-link
        >
        <!-- CONNECTION BOUTON -->
        <input id="submit_btn" type="submit" value="Connexion" />
      </div>
    </form>

    <div v-if="this.status == 200">
      <p>Vous venez de recevoir un mail pour valider votre compte !</p>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      status: "",
      role: "",
    };
  },

  methods: {
    //Demande asynchrone permettant la récupération des identifiants utilisateur via l'API
    async connectUser() {
      const url = "http://127.0.0.1:8000/api/login";
      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
        body: JSON.stringify({
          email: this.email,
          password: this.password,
        }),
      };

      const response = await fetch(url, options);
      const data = await response.json();

      this.status = data.status_code;
      this.role = data.role;

      // Sauvegarde du token généré par l'API lors de la connection
      if (data.status_code === 200) {
        localStorage.setItem("@token", data.access_token);
        localStorage.setItem("@id", data.id);
        if (this.role == "admin") {
          this.$router.push({ name: "Dashboard" });
        } else {
          this.$router.push({ name: "DashMember" });
        }
      }
    },

    async SendMagicLink() {
      const urlMagicLink = "http://127.0.0.1:8000/api/login";

      const optionsMagicLink = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: "Bearer " + localStorage.getItem("@token"),
          Accept: "application/json",
        },

        body: JSON.stringify({
          email: this.email,
        }),
      };

      const responseMagicLink = await fetch(urlMagicLink, optionsMagicLink);

      const dataMagicLink = await responseMagicLink.json();
      console.log(dataMagicLink);

      this.status = dataMagicLink.status_code;
    },
  },
  watch: {
    role(value) {
      this.setRole(value);
    },
  },
  inject: ["setRole"],
};
</script>
<style scoped>
.form_connexion {
  width: 40%;
  height: 60%;
  margin: auto;
}
.items_form {
  display: flex;
  flex-direction: column;
  justify-items: center;
  justify-content: space-between;
  width: 100%;
}
.p_red {
  color: red;
}

input {
  width: 100%;
  height: 20%;
  margin: 2% 0%;
  border: none;
  border-radius: 5px;
  box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
  outline: none;
  transition: box-shadow 1.2s;
}
input:focus {
  box-shadow: inset 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
  outline: none;
}
label {
  height: 20%;
  text-align: left;
  margin: 2% 0%;
}
#submit_btn {
  width: 25%;
  color: #0f0f0f;
  background: #db9024;
  cursor: pointer;

  transition: background 1s;
  height: 40px;
  box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
  margin-left: 38%;
}
#submit_btn:hover {
  color: #0f0f0f;
  transition: box-shadow 1s;
  box-shadow: inset 3px 3px 3px 2px rgba(0, 0, 0, 0.2);
}

.img_container img {
  width: 20%;
  margin-left: 40%;
  margin-bottom: 10%;
}
</style>