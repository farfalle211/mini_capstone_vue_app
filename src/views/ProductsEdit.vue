<template>
  <div class="products-edit">
    <h1>Edit Recipe</h1>

    <div>
      <h4>Edit Product</h4>
    </div>

    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>

    <form v-on:submit.prevent="submit()">

    <div>
      Name: <input v-model="product.name">
      Price: <input v-model="product.formatted.price">
      Description: <input v-model="product.description">
      Image Url: <input v-model="product.image_url">
    </div>

      <input type="submit" value="Update" class="btn btn-warning">
      
    </form>
  </div>
</template>

<style>
</style>

<script>
  var axios = require('axios');

export default {
  data: function() {
    return {
      product: {
                id: "",
                name: "",
                description: "",
                image_url: "",
                price: "",
                tax: "",
                total: "",
                formatted:{
                  price: "",
                  tax: "",
                  total: ""}
              }
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id)
      .then(response => { 
        console.log(response.data);
        this.product = response.data;
      });
  },
  methods: {
    submit: function() {
      var params = {
                    name: this.product.first_name,
                    description: this.product.description, 
                    image_url: this.product.image_url,
                    price: this.product.price
                    };

              axios.patch("/api/products/" + this.product.id, params)
                    .then(response => {
                    this.$router.push("/products/" + this.product.id);
                  }).catch(error => {
                    this.errors = error.response.data.errors;
                  });
          }
    }
    
};
</script>