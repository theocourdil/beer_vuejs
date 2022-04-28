<template>
    <div class="body">
        <DetailsBeer :beer="beer"/>
    </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
import DetailsBeer from '@/components/DetailsBeer';

Vue.use(VueAxios, axios)

export default {
    components: {
        DetailsBeer
    },
    data () {
        return {
            beer: []
        }
    },
    mounted () {
        Vue.axios.get('https://api.punkapi.com/v2/beers/' + this.$route.params.id)
        .then((resp) => {
            this.beer = resp.data[0]
        })
    }
}
</script>
