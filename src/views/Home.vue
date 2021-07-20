<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Click the button below!</h2>
    <button v-on:click="changeColor" v-bind:class="{ blue: blue }">{{ blue }}</button>
    <h2>Create a new product!</h2>
    <p>Name:</p>
    <input type="" v-model="newProductParams.name" />
    <p>Description:</p>
    <input type="" v-model="newProductParams.description" />
    <p>Price:</p>
    <input type="" v-model="newProductParams.price" />
    <p>Image URL:</p>
    <input type="" v-model="newProductParams.image_url" />
    <p></p>
    <button v-on:click="createProduct">Create</button>
    <div v-for="product in products" v-bind:key="product.id">
      <p></p>
      {{ product.name }} - ${{ product.price }}
      <p></p>
      <img v-bind:src="product.image_url" alt="No image found" />
    </div>
  </div>
</template>
<style>
button {
  padding: 16px;
}
.blue {
  background-color: lightskyblue;
}
img {
  width: 200px;
}
</style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      blue: true,
      products: [],
      newProductParams: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    changeColor: function () {
      this.blue = !this.blue;
    },
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
        console.log(response.data);
      });
    },
    createProduct: function () {
      axios.post("http://localhost:3000/products", this.newProductParams).then((response) => {
        this.products.push(response.data);
        console.log(response.data).catch((error) => console.log(error.response));
      });
    },
  },
};
</script>
