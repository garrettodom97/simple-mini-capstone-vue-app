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
      <p></p>
      <button v-on:click="updateProductShow(product)">Update</button>
      <button v-on:click="showProduct(product)">More Info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <img :src="currentProduct.image_url" alt="No Image Found" />
        <p></p>
        <button>Close</button>
      </form>
    </dialog>
    <dialog id="product-update">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>
          Name:
          <input type="text" v-model="currentProduct.name" />
        </p>
        <p>
          Price:
          <input type="text" v-model="currentProduct.price" />
        </p>
        <p>
          Description:
          <input type="text" v-model="currentProduct.description" />
        </p>
        <img :src="currentProduct.image_url" alt="No Image Found" />
        <p></p>
        <button v-on:click="updateProduct(currentProduct)">Update</button>
        <button>Close</button>
      </form>
    </dialog>
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
      currentProduct: {},
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
    showProduct: function (product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProductShow: function (product) {
      this.currentProduct = product;
      document.querySelector("#product-update").showModal();
    },
    updateProduct: function (product) {
      axios.patch("http://localhost:3000/products/" + product.id, product).then((response) => {
        console.log("Success!", response.data);
      });
    },
  },
};
</script>
