<template>
  <div class="body">
    <div class="about">
      <h1>Voici nos bières</h1>
    </div>
    <input placeholder="Rechercher par nom" v-model="searchQuery">
    <b-pagination
      v-model="currentPage"
      :total-rows="nbBeers"
      :per-page="perPage"
      aria-controls="my-table"
    ></b-pagination>
    <p class="mt-3">Page: {{ currentPage }}</p>
    <div v-for="beer of beers" :key="beer.name" :per-page="perPage" :current-page="currentPage">
      <div class="container-fluid">
        <div class="card shadow-sm mb-3">
          <div class="p-3 fw-bold">{{beer.name}}</div>
          <td>{{beer.description}}</td>
          <td><b>Température de fermentation :</b> {{beer.method.fermentation.temp.value}} °C</td>
          <img :src=beer.image_url width="50px">
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
      perPage: 10,
      currentPage: 1,
      searchQuery: '',
      beers: [],
      nbBeers: 325
    }
  },
  mounted () {
    Vue.axios.get('https://api.punkapi.com/v2/beers?per_page=' + this.perPage)
    .then((resp)=>{
      this.beers=resp.data
      console.log(resp.data);
    })
  },
  computed: {

  },
  watch: {
    currentPage() {
      Vue.axios.get('https://api.punkapi.com/v2/beers?per_page=' + this.perPage + '&page=' + this.currentPage)
      .then((resp)=>{
        this.beers = resp.data
      })
    },
    searchQuery() {
      console.log(this.searchQuery);
      let match = '';
      if (this.searchQuery != '') {
        match = `&beer_name=${this.searchQuery}`
      }
      Vue.axios.get('https://api.punkapi.com/v2/beers?per_page=' + this.perPage + match)
      .then((resp)=>{
        this.beers = resp.data
        this.currentPage = 1;
        this.nbBeers = this.beers.length;
        if (this.searchQuery == '') {
          this.nbBeers = 325;
        }
      })
    }
  }
}
</script>