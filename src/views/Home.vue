<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>New Product Form</h1>
    <div>
      name: <input type="text" v-model="newProductName">
      price: <input type="text" v-model="newProductPrice">
      description: <input type="text" v-model="newProductDescription">
      imageUrl: <input type="text" v-model="newProductImageUrl">
    </div>
    <button v-on:click="createProduct()"> Submit Product </button>
    <div>
      <img src="https://media.giphy.com/media/ensk4bvNQhoBbHCwZa/giphy.gif">
    </div>
    <div v-for="product in products">    
      <h2>{{ product.name}}</h2>
      <p>Description: {{ product.description }}</p>
      <img v-bind:src="product.image_url">
    </div>
          <!-- <p>{{ products }}</p> -->
  </div>
</template>

<style>
</style>

<script>
let axios = require("axios");
// import axios from "axios";

export default {
  data: function() {
    return {
      message: "You're a Champ!!",
      products: [],
      newProductName:"",
      newProductPrice:"",
      newProductDescription:"",
      newProductImageUrl:""
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    })
  },
  methods: {
    createProduct: function() {
      console.log("Product Created!");

      let params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        image_url: this.newProductImageUrl
      }

      axios.post('api/products', params).then(response => {
        // console.log(response.data);
        this.products.push(response.data); 
      }).catch(error => {
        console.log(error.response);
      });
    }
  }
};
</script>
