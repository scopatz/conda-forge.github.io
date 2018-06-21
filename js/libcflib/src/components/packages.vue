<template>
<div id="vuepackages" class="container">
  <div class="row">Package Name:</div>
  <input type="text" class="form-control" v-model="filterkey">

  <table class="table table-hover">
    <thead>
      <tr>
        <th><a href="#" v-on:click="sortvia('package.name')">name</a></th>
        <th><a href="#">arch</a></th>
      </tr>
    </thead>

    <tbody>
      <!-- <tr v-for="p of packages | filterBy filterkey | orderBy sortparam order"> -->
      <tr v-for="pkg of packages" v-bind:key=pkg.name >
        <td>{{ pkg.name }}</td>
        <td>{{ pkg.name }}</td>
      </tr>
    </tbody>
  </table>
</div>
</template>

<script>
import axios from 'axios';
import Vue from 'vue';

export default new Vue({
    el: '#vuepackages',

    data: {
        sortparam: '',
        filterkey: '',
        order: 1,
        errors: [],
        packages: {}
    },

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

    methods: {
        sortvia: function (sortparam, order) {
        this.order = order * -1;
            this.sortparam = sortparam;
        }
    }
})
</script>
