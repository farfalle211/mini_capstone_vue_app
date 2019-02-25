<template>
  <div class="products-index">

      <h1>All Products</h1>
      <div>
        Filter Title: <input v-model="titleFilter" list="products-names">

        <datalist id="products-names">
          <option v-for="product in products">{{product.name}}</option>
        </datalist>
      </div>
      <div v-for="product in filterBy(products, titleFilter, 'name')">
        <h2>{{ product.name }}</h2>
        <router-link v-bind:to="'/products/' + product.id">
          <img v-bind:src="product.image_url" v-bind:alt="product.name">
        </router-link>
      </div>
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
  import Vue2Filters from "vue2-filters";

export default {
  data: function() {
    return {
      products:[], 
      currentProduct: {},
      titleFilter: ''
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

  },
  mixins: [Vue2Filters.mixin]
};
</script>