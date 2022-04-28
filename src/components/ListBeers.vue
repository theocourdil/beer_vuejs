<template>
    <div>
        <div v-for="beer of beers" :key="beer.name">
            <router-link :to="{ name: 'oneBeer', params: { id: beer.id }}">
                <div class="container-fluid">
                    <div class="card shadow-sm mb-3">
                    <div class="p-3 fw-bold">{{beer.name}}</div>
                    <td>{{beer.description}}</td>
                    <td><b>Température de fermentation :</b> {{beer.method.fermentation.temp.value}} °C</td>
                    <td><b>EBC :</b> {{beer.ebc}}</td>
                    <img :src=beer.image_url width="50px">
                    </div>
                </div>
            </router-link>
        </div>
    </div>
</template>

<script>
export default {
  props: ['beers'],
}
</script>

<style scoped>
a {
    text-decoration: none;
    color: inherit;
}

a .card:hover {
    background: #2c3e50;
    color: #ddd;
    transition: all .3s ease-in-out;
}
</style>