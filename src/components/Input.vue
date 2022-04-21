<template>
    <input placeholder="Rechercher par nom" v-model="searchQuery">
</template>

<script>
export default {
  data(){
    return{
      searchQuery: null,
    }
  }
}
</script>