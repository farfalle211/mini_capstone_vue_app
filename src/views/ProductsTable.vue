<template>
  <div class="products-index">

      <h1>All Products</h1>
      <div>
        Filter Title: <input v-model="titleFilter" list="products-names">

        <datalist id="products-names">
          <option v-for="product in products">{{product.name}}</option>
        </datalist>

      <table class="table table-striped table-dark">
          <thead class="thread-light">
           <tr>
             <th v-on:click="setSortAttribute('id')">ID
              <span v-if="sortAttribute === 'id' && sortOrder === 1"> ^</span>
              <span v-else-if="sortAttribute === 'id' && sortOrder === -1"> v</span>
              <span v-else></span>
            </th>
             <th v-on:click="setSortAttribute('name')">Name {{ orderIndicator('title') }}</th> 
             <th v-on:click="setSortAttribute('description')">Description {{ orderIndicator('description') }}</th>
             <th v-on:click="setSortAttribute('price')">Price {{ orderIndicator('price')}}</th>
             <th v-on:click="setSortAttribute('tax')">Tax {{ orderIndicator('tax')}}</th>
             <th v-on:click="setSortAttribute('total')">Total {{ orderIndicator('total')}}</th>
           </tr>
         </thead>
         <tbody>
           <tr v-for="product in orderBy(filterBy(products, titleFilter, 'name'), sortAttribute, sortOrder)">
             <td>
               {{ product.id }}
             </td>
             <td>
               <router-link v-bind:to="'/products/' + product.id">
                 {{ product.name }}
               </router-link>
             </td>
             <td>
               {{ product.description }}
             </td>
             <td>
               {{ product.price }}
             </td>
             <td>
               {{ product.tax }}
             </td>
             <td>
               {{ product.total }}
             </td>
           </tr>
         </tbody>
       </table>


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
      titleFilter: '',
      sortAttribute: '',
      sortOrder: 1
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
    setSortAttribute: function(inputAttribute) {
      if (this.sortAttribute = inputAttribute) {
        this.sortOrder *= -1;
      } else {
        this.sortAttribute = inputAttribute;
        this.sortOrder = 1;
      }
    },
    orderIndicator: function(inputAttribute) {
      if (this.sortAttribute === inputAttribute) {
        if (this.sortOrder === 1) {
          return "v";
      
        } else {
          return "^";
        }
      } else {
        return " ";
      }
    }
  },
  mixins: [Vue2Filters.mixin]
};
</script>