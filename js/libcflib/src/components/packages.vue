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
        <!-- <td>{{ arches[pkg.name].join(', ') }}</td> -->
        <!-- <td>{{ arches() }}</td> -->
        <td v-for="arch in uniqueArches(pkg.name)" v-bind:key="arch + 'kjsjs'">{{ arch.keys()}} </td>
        <!-- <td>{{ uniqueArches(pkg.name) }}</td> -->

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
    packages: {},
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
    },
  },

  methods: {
    arches() {
      // Normalize arch data on fetch
      var pkg;
      var self = this;
      var arcs = {};
      var pkgArches = new Set([]);
      if (Object.keys(self.packages).length === 0) {
        return arcs;
      }
      for (pkg in Object.values(self.packages)) {
        pkgArches = new Set([]);
        var channel, a;
        for (channel in Object.values(pkg.artifacts)) {
          for (a in Object.keys(channel)) {
            pkgArches.add(a);
          }
        }
        arcs[pkg.name] = Array.from(pkgArches)
      }
      return arcs;
    },
    uniqueArches(name) {
      // Normalize arch data on fetch
      if (!(name in this.packages)) {
        return [];
      }
      var pkg = this.packages[name];
      //return pkg;
      //return Object.values(pkg);
      //return Object.values(pkg.artifacts);
      var channels = Object.values(pkg.artifacts);
      return channels;
      var pkgArches = new Set([]);
      for (var channel in channels) {
      //for (var channel in Object.values(pkg.artifacts)) {
        return [channel];
        for (var arch in Object.keys(channel)) {
          pkgArches.add(arch);
        }
      }
      return Array.from(pkgArches);
    },
  }
}
</script>
