<template>
   <div class="products-show">
    <img v-bind:src="product.image_url" :alt="product.name">
    <h1>{{ product.name }}</h1>
    <h4>Description: {{ product.description }}</h4>
    <h4>Image Url: {{ product.image_url }}</h4>
    
    <p>Price: {{ product.formatted.price }}</p>
    <p>Tax: {{ product.formatted.tax }}</p>
    <p>Total: {{ product.formatted.total }}</p>



    <router-link :to=" '/products/' + product.id + '/edit' ">Edit</router-link>
  
    <button v-on:click="destroyProduct()">Delete</button>

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
      axios.get("/api/products/" + this.$route.params.id )
      .then(response => {
        console.log(response.data);
        this.product = response.data;
      });
    },
    methods: {
       destroyProduct: function (inputProduct) {
      axios.delete("api/products/" + inputProduct.id)
        .then(response => {
          console.log("Success", response.data);
          var index = this.products.indexOf(inputProduct);
          this.products.splice(index,1);
        });
      }
    }
  };

</script>