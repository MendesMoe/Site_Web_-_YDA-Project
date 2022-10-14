<template>
  <div class="content-page-services">
    <h2 class="title-page">Services</h2>
    <!--component de selection du type-->
    <SelectType @change="getOptionValue" />
    <!--v-for pour afficher tout les services en BDD -->
    <div class="groupe_service_card">
      <Service
        v-for="(element, index) in filterTypeId"
        :key="index"
        :values="element"
      ></Service>
    </div>
  </div>
</template>
<script>
/*import du composant product*/
import Service from "./Service.vue";
import SelectType from "../UI/SelectTypes.vue";

export default {
  inject: ["role"],
  emits: [],

  data() {
    return {
      servicesArray: [],
      id: "",
      type_id: "",
      getValueFromOptions: "",
    };
  },
  components: {
    Service,
    SelectType,
  },

  updated() {
    console.log(this.filterTypeId);
  },

  async mounted() {
    /*requete pour récuperer au montage tout les services en BDD*/
    const url = "http://127.0.0.1:8000/api/services";

    const options = {
      method: "GET",

      headers: {
        "Content-Type": "application/json",
        Authorization: "Bearer " + localStorage.getItem("@token"),
        Accept: "application/json",
      },
    };
    const response = await fetch(url, options);
    const data = await response.json();
    console.log("SERVICES API");
    console.log(data.donnees);

    if (Array.isArray(data.donnees)) {
      this.servicesArray = data.donnees;
    }
  },
  /* method au click pour selectionner le bon service avec le bon ID qui renvoie vers la page des produits de ce service selectionné */

  methods: {
    /*récupération de l'event change sur le select pour la fonction de filtre ci dessous*/
    getOptionValue(event) {
      this.getValueFromOptions = event.target.value;
    },
  },
  computed: {
    /* fonction de filtre par type de services*/

    filterTypeId() {
      return this.servicesArray.filter((element) => {
        if (this.getValueFromOptions != "") {
          return String(this.getValueFromOptions) == String(element.type_id);
        } else {
          return true;
        }
      });
    },
  },
};
</script>

<style scoped>
.content-page-services {
  width: 100%;
  height: 100%;
  padding: 2% 2%;
  justify-content: center;
  text-align: center;
}
.title-page {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
.groupe_service_card {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin: 50px 0;
}

.anim {
  width: 25%;
  color: #0f0f0f;
  background: #db9024;
  cursor: pointer;
  height: 40px;
  box-shadow: 2px 2px 2px 1px rgb(0 0 0 / 20%);
  text-decoration: none;
  margin: 2%;
}
</style>