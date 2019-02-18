<template>
  <div class="home">
     <h1>New Product</h1>
    <div>
      <div>
        Name: <input v-model="newProductName">
      </div>
       <div>
        Price: <input v-model="newProductPrice">
      </div>
       <div>
        Description: <input v-model="newProductDescription">
      </div>
       <div>
        Image URL: <input v-model="newProductImageUrl">
      </div>
      <button v-on:click="createProduct()">Create</button>
    </div>


    <h1>All Products</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.name">
      <p>Price: {{ product.formatted.price }}</p>
    </div>
  </div>
</template>

<style> 
/*this is where the CSS goes*/
  img {
    width: 350px;
  }
</style>

<script>
  var axios = require('axios');

export default {
  data: function() {
    return {
      products:[], 
      newProductName:"",
      newProductPrice:"",
      newProductDescription:"",
      newProductImageUrl:""
    };
  },
  created: function() {
    axios.get("/api/products")
      .then(response => { 
        this.products = response.data;  //.data just is pulling out the JSON core from the response (response is meta information and JSON body)...this is unlike http gem where .parse then converts it to Ruby array of hashes.
         //This is redefining the products array here with the index JSON from the api
      });
  },
  methods: {
    createProduct: function() {
      console.log("Create the Product...");
       var params = {
                    name: this.newProductName,
                    price: this.newProductPrice,
                    description: this.newProductDescription,
                    image_url: this.newProductImageUrl
                    };
      axios.post("/api/products", params)
        .then(response => {
          console.log("Success", response.data);
          this.products.push(response.data);
        });
    }
  }
};
</script>