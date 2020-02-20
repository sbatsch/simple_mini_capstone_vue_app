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
    <div>
      <h6> And here are the products... </h6>
    </div>
    <div class="show-page" v-for="product in products">    
      <img v-on:click="showProduct(product)" v-bind:src="product.image_url">
      <h2>{{ product.name}}</h2>
      <div v-if="product == currentProduct"> 
      <p>Description: {{ product.description }}</p>
      <div class="edit-form"> 
        <h4>Edit Product</h4>

        <div>
          name: <input type="text" v-model="product.name">
        </div>

        <div>
          price: <input type="text" v-model="product.price">
        </div>

        <div>
          description: <input type="text" v-model="product.description">
        </div>

        <div>
          image url: <input type="text" v-model="product.image_url">
        </div>

        <button v-on:click="updateProduct(product)"> Update </button>

      </div>
      <button v-on:click="destroyProduct(product)"> Delete </button>
    </div>

    </div>
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
      currentProduct: {}, 
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
      };

      axios.post('api/products', params).then(response => {
        // console.log(response.data);
        this.products.push(response.data); 
      }).catch(error => {
        console.log(error.response);
      });
    },

    showProduct: function(product) {
      if (this.currentProduct !== product) {
      this.currentProduct = product; 
      } else {
        this.currentProduct = {};
      };
    },

    updateProduct: function(inputProduct) {
      var clientParams = {
        name: inputProduct.name,
        price: inputProduct.price,
        description: inputProduct.description,
        image_url: inputProduct.image_url
      };

      axios.patch("/api/products/" + inputProduct.id, clientParams)
      .then(response => {
        console.log("Success", response.data);
      }).catch(error => {
        console.log(error.response);
      });
    },

    destroyProduct: function(inputProduct) {
      axios.delete("/api/products/" + inputProduct.id).then(response => {
        console.log("Success", response.data);
        var index = this.products.indexOf(inputProduct);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>
