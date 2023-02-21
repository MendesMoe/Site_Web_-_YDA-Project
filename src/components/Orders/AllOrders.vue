<template>
  <div class="content-page-order">
    <h2 class="title-page">Commandes en attente</h2>
    <!--v-for pour afficher tout les commandes en BDD -->
    <table>
      <thead>
        <tr>
          <div>
            <th>N°</th>
            <th>
              <span>Entreprises </span>
              <SelectFirms @change="getFirmValue($event)" />
            </th>
            <th>Salarié</th>
            <th>Total</th>
            <th>Date création</th>
            <th>Date dernière modification</th>
            <th>
              <select
                name="status"
                class="select-component"
                id="status"
                @change="selectedStatus = $event.target.value"
              >
                <option value="">Statuts</option>
                <option value="en cours">En cours</option>
                <option value="en attente">En attente</option>
                <option value="terminée">Terminée</option>
                <option value="annulee">Annulée</option>
              </select>
            </th>
          </div>
        </tr>
      </thead>
      <!-- affichage des resultats -->

      <tbody v-for="(element, index) in filterFirms" :key="index">
        <tr
          id="tr-body-orders"
          v-for="(user, index) in element.users"
          :key="index"
        >
          <div v-for="(order, index) in user.orders" :key="index">
            <td>
              {{ order.id }}
            </td>
            <td class="selectfirm" @click="getOrdersByFirm(element.id)">
              {{ element.name }}
            </td>
            <td>{{ user.firstname }} {{ user.lastname }}</td>
            <td>{{ order.total }}€</td>
            <td>
              {{ moment(order.created_at).format("DD MMM YYYY, HH:mm ") }}
            </td>
            <td>
              {{ moment(order.updated_at).format("DD MMM YYYY, HH:mm ") }}
            </td>
            <td>{{ order.status }}</td>
          </div>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import SelectFirms from "../UI/SelectFirms.vue";

import moment from "moment";
export default {
  data() {
    return {
      ordersList: [],
      id: "",
      status: "",
      getValueFromOptions: "",
      idFirm: "",
      selectedStatus: "",
    };
  },
  components: {
    SelectFirms,
  },

  created: function () {
    this.moment = moment;
  },

  async mounted() {
    /*requete pour récuperer au montage tout les entreprises, et les users avec leurs commandes en BDD*/
    const url = "http://127.0.0.1:8000/api/firms";
    //Options de la requête API
    const options = {
      method: "GET",
      headers: {
        Authorization: "Bearer " + localStorage.getItem("@token"),
        Accept: "application/json",
      },
    };
    // va chercher les options de l'API
    const response = await fetch(url, options);
    // la récupération des data stockées dans l'API
    const data = await response.json();
    this.ordersList = data[0];
    console.log("ordersLIST in AllOrders");
    console.log(this.ordersList);
    console.log(data[0]);
  },
  methods: {
    /*récupération de l'event change sur le select pour la fonction de filtre ci dessous*/
    getFirmValue(event) {
      this.getValueFromOptions = event.target.value;
      console.log("getValueFromOptions");
      console.log(this.getValueFromOptions);
    },
    async getOrdersByFirm(id) {
      this.$router.push({
        name: "Users",
        params: { firmId: id },
      });
    },
  },

  computed: {
    /* fonction de filtre par status*/

    filterFirms() {
      let filteredFirms = this.ordersList.filter((element) => {
        console.log("order list a nouveau");
        console.log(this.ordersList);
        if (this.getValueFromOptions != "") {
          return String(this.getValueFromOptions) == String(element.name);
        } else {
          return true;
        }
      });

      if (this.selectedStatus !== "") {
        filteredFirms = filteredFirms.map((firm) => {
          let firmCopy = JSON.parse(JSON.stringify(firm));
          firmCopy.users = firmCopy.users.map((user) => {
            user.orders = user.orders.filter((order) => {
              return order.status === this.selectedStatus;
            });
            return user;
          });
          return firmCopy;
        });
      }

      return filteredFirms;
    },
  },
};
</script>

<style scoped>
.content-page-order {
  width: 100%;
  height: 100%;
  padding: 2% 2%;
  justify-content: center;
  text-align: center;
}
.title-page {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
.selectfirm:hover {
  cursor: pointer;
  border: #ffa500 solid 1px;
}
.select-component {
  font-size: 16px;
  padding: 2px;
  margin-top: 5px;
  border-radius: 9px;
  margin-bottom: 5px;
}
.order_card {
  display: flex;
  flex-direction: column;
  margin: 2%;
  border-radius: 5px;
  border: 5px solid black;
  padding: 1%;
}
.groupeOrders {
  display: flex;
}
table {
  width: 90%;
  height: 90%;
  align-items: center;
  overflow-y: scroll;
  overflow-x: scroll;
}
table tr {
  transition: all 0.2s ease-in;
  background-color: #fff;
  cursor: pointer;
  width: 100%;
  align-items: center;
}
table th,
td {
  padding: 5px;
  border-bottom: 1px solid #ddd;
  border-right: 1px solid #ddd;
  min-width: 180px;
  max-height: 200px;
  flex-direction: column;
}

table thead tr {
  background-color: #f39c11;
  font-weight: bold;
  color: #fff;
  width: 100%;
}

table tbody {
  width: 100%;
}

table tbody {
  min-width: 180px;
  max-height: 200px;
}

.white {
  background-color: white;
}
.gray {
  background-color: "F4F4F4";
}
</style>
