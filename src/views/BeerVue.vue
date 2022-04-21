<template>
  <div class="body">
    <div class="about">
      <h1>Voici nos bières</h1>
    </div>
    <input placeholder="Rechercher par nom" v-model="searchQuery">
    <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      aria-controls="my-table"
    ></b-pagination>
    <p class="mt-3">Page: {{ currentPage }}</p>
    <div v-for="r of resultQuery" :key="r" :per-page="perPage" :current-page="currentPage">
      <div class="container-fluid">
        <div class="card shadow-sm mb-3">
          <div class="p-3 fw-bold"> Nom: {{r.name}}</div>
          <td>Description: {{r.description}}</td>
          <td>Température de fermentation: {{r.method.fermentation.temp.value}} °C</td>
          <img :src=r.image_url width="50px">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios, axios)
export default {
  data () {
    return {
      perPage: 3,
      currentPage: 1,
      searchQuery: null,
      beers: [],
    }
  },
  mounted () {
    Vue.axios.get('https://api.punkapi.com/v2/beers')
    .then((resp)=>{
      this.beers=resp.data
      console.warn(resp.data)
    })
  },
  computed: {
    resultQuery() {
      if (this.searchQuery) {
        return this.beers.filter(item => {
          return this.searchQuery
            .toLowerCase()
            .split(" ")
            .every(v => item.name.toLowerCase().includes(v));
        });
      } else {
        return this.beers;
      }
    },
    rows() {
        return this.beers.length
      }
  },
}
</script>