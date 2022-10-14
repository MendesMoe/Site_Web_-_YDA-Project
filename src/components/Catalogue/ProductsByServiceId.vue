<template>
  <div class="content-page-products-by-service">
    <h1>Produits</h1>
    <div v-for="(element, index) in productsArray" :key="index">
      <Product
        v-for="(value, index) in element.products"
        :key="index"
        :values="value"
        :addToCart="addToCart"
      />
    </div>
  </div>
</template>

<script>
import Product from "./Product.vue";

export default {
  components: {
    Product,
  },
  props: {
    servicesId: String,
    addToGlobal: Function,
  },
  data() {
    return {
      productsArray: [],
      name: "",
      cart: [],
    };
  },
  async mounted() {
    const url = `http://127.0.0.1:8000/api/services/${this.servicesId}`;

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
    this.productsArray = data.donnees;

    let cart = JSON.parse(localStorage.getItem("@cart"));
    if (Array.isArray(cart)) {
      console.log("PRoduct by service CART");
      this.cart = cart;
    }
  },

  methods: {
    addToCart(product) {
      this.cart = [...this.cart, product];
      localStorage.setItem("@cart", JSON.stringify(this.cart));
    },
  },
};
</script>

<style scoped>
.content-page-products-by-service {
  width: 100%;
  height: 100%;
  padding: 2% 2%;
  justify-content: center;
  text-align: center;
}
img {
  width: 15rem;
  height: 15rem;
}

.image {
  margin-left: 1%;
}

.img_parent {
  margin: auto;
  display: flex;
  gap: 20%;
}
.img_container {
  display: flex;
  flex-direction: column;
}
.img_container img {
  width: 200px;
  height: auto;
  margin: 0px;
}

.edit_product {
  display: flex;
  flex-direction: column;
  margin-left: 24%;
}
</style>