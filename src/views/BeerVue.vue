<template>
  <div class="body">
    <div class="about">
      <h1>Voici nos bières</h1>
    </div>
    <input placeholder="Rechercher par nom" v-model="searchQuery">
    <div class="my-3">
      EBC entre ... et ...
    </div>
    <div class='range-slider'>
      <input type="range" min=1 max=140 step="1" v-model="minEBC">
      <input type="number" min=1 max=140 step="1" v-model="minEBC">
      <input type="range" min=1 max=140 step="1" v-model="maxEBC">
      <input type="number" min=1 max=140 step="1" v-model="maxEBC">
  </div>
    <p class="mt-3">Page: {{ currentPage }}</p>
    <ListBeers :beers="beers" />
    <b-pagination
      v-model="currentPage"
      :total-rows="nbBeers"
      :per-page="perPage"
      aria-controls="my-table"
    ></b-pagination>
  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
import ListBeers from '@/components/ListBeers';
Vue.use(VueAxios, axios)
export default {
  components: {
    ListBeers
  },
  data () {
    return {
      perPage: 10,
      currentPage: 1,
      searchQuery: '',
      beers: [],
      nbBeers: 325,
      minEBC: 1,
      maxEBC: 140,
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
        this.beers = resp.data;
      })
    },
    searchQuery() {
      this.minEBC = 1;
      this.maxEBC = 140;
      let match = '';
      if (this.searchQuery != '') {
        match = `&beer_name=${this.searchQuery}`
      }
      Vue.axios.get('https://api.punkapi.com/v2/beers?per_page=' + this.perPage + match)
      .then((resp)=>{
        this.beers = resp.data;
        this.currentPage = 1;
        this.nbBeers = this.beers.length;
        if (this.searchQuery == '') {
          this.nbBeers = 325;
        }
      })
    },
    minEBC() {
      this.updateByTemp();
    },
    maxEBC() {
      this.updateByTemp();
    }
  },
    methods: {
    slider: function() {
      if (this.minEBC > this.minEBC) {
        var tmp = this.maxPH;
        this.maxPH = this.minEBC;
        this.minEBC = tmp;
      }
    },
    updateByTemp() {
      console.log("min :" + this.minEBC);
      console.log("max :" + this.maxEBC);

      Vue.axios.get('https://api.punkapi.com/v2/beers?per_page=' + this.perPage + '&ebc_gt=' + this.minEBC + '&ebc_lt=' + this.maxEBC)
      .then((resp)=>{
        this.beers = resp.data;
        this.currentPage = 1;
        this.nbBeers = this.beers.length;
      })
    }
  }
}
</script>

<style scoped>
.range-slider {
  width: 300px;
  margin: auto;
  text-align: center;
  position: relative;
  height: 6em;
}

.range-slider input[type=range] {
  position: absolute;
  left: 0;
  bottom: 0;
}

input[type=number] {
  border: 1px solid #ddd;
  text-align: center;
  font-size: 1.6em;
  -moz-appearance: textfield;
}

input[type=number]::-webkit-outer-spin-button,
input[type=number]::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

input[type=number]:invalid,
input[type=number]:out-of-range {
  border: 2px solid #ff6347;
}

input[type=range] {
  -webkit-appearance: none;
  width: 100%;
}

input[type=range]:focus {
  outline: none;
}

input[type=range]:focus::-webkit-slider-runnable-track {
  background: #2497e3;
}

input[type=range]:focus::-ms-fill-lower {
  background: #2497e3;
}

input[type=range]:focus::-ms-fill-upper {
  background: #2497e3;
}

input[type=range]::-webkit-slider-runnable-track {
  width: 100%;
  height: 5px;
  cursor: pointer;
  animate: 0.2s;
  background: #2497e3;
  border-radius: 1px;
  box-shadow: none;
  border: 0;
}

input[type=range]::-webkit-slider-thumb {
  z-index: 2;
  position: relative;
  box-shadow: 0px 0px 0px #000;
  border: 1px solid #2497e3;
  height: 18px;
  width: 18px;
  border-radius: 25px;
  background: #a1d0ff;
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -7px;
}
</style>