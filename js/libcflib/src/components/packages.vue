<template>
<div id="vuepkgs" class="container">
  <div class="row">Package Name:</div>
  <input type="text" class="form-control" v-model="filterkey">

  <table class="table table-hover">
    <thead>
      <tr>
        <th><a href="#" v-on:click="sortvia('pkg.name')">name</a></th>
        <th><a href="#">arch</a></th>
      </tr>
    </thead>

    <tbody>
      <!-- <tr v-for="p in packages | filterBy filterkey | orderBy sortparam order"> -->
      <tr v-for="pkg in filteredPackages" v-bind:key="pkg.name" >
        <td>{{ pkg.name }}</td>
        <td>{{ pkg.name }}</td>
      </tr>
    </tbody>
  </table>
</div>
</template>

<script>
//import Vue from 'vue/dist/vue.esm.browser.js';
import axios from 'axios';

export default {
  data() { return {
    sortparam: "",
    filterkey: "",
    order: 1,
    errors: [],
    packages: {}
    }},

  // Fetches posts when the component is created.
  created() {
    axios.get('http://35.225.70.78:80/packages')
    .then(response => {
      this.packages = response.data
    })
    .catch(e => {
      this.errors.push(e)
    })
  },

  computed: {
    filteredPackages() {
      var self = this;
      return Object.values(self.packages).filter(function(value) {
        return value.name.indexOf(self.filterkey) > -1;
        })
    }
  }
}
</script>
