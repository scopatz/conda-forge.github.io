<template>
<div id="vuepkgs" class="container">
  <div class="row">Package Name:</div>
  <input type="text" class="form-control" v-model="filterkey">

  <table class="table table-hover">
    <thead>
      <tr>
        <th>names</th>
        <th>channels</th>
        <th>archs</th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="pkg in filteredPackages" v-bind:key="pkg.name + '5a10b298'">
        <td>{{ pkg.name }}</td>
        <td>{{ Object.keys(pkg.artifacts).join(', ') }}</td>
        <td>{{ pkg.channel }}</td>
      </tr>
    </tbody>
  </table>
</div>
</template>

<script>
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
