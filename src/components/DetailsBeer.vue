<template>
    <div class="flex">
        Nom : {{ beer.name }}
        Tagline : {{ beer.tagline }}
        First brewed : {{ beer.first_brewed }}
        Description : {{ beer.description }}
        <div>
          <img :src=beer.image_url class="image">
        </div>
        ABV : {{ beer.abv }}
        IBU : {{ beer.ibu }}
        Target FG : {{ beer.target_fg }}
        Target OG : {{ beer.target_og }}
        EBC : {{ beer.ebc }}
        SRM : {{ beer.srm }}
        PH : {{ beer.ph }}
        Volume : {{ beer.volume.value }} {{ beer.volume.unit }}
    </div>
</template>

<script>
export default {
  props: ['beer'],
}
</script>

<style scoped>

.flex {
  display: flex;
  flex-direction: column;
}

.image {
  height: 300px;
  margin: 20px auto;
}
</style>