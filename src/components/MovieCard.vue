<script setup>
defineProps(['detail'])
</script>

<template>
  <div style="cursor: pointer" @click="$router.push({ path: `${detail.id}` })">
    <img
      alt="Movie poster"
      :src="`https://image.tmdb.org/t/p/w400/${detail.poster_path}`"
      style="position: relative"
    />
    <span class="rating text-bold text-white">{{ detail.vote_average.toFixed(1) }}</span>
    <div class="text-white">
      {{ detail.title }}
    </div>
    <div class="text-grey">{{ detail.release_date.substring(0, 4) }}</div>
  </div>
</template>

<style scoped>
.rating {
  position: relative;
  top: -50px;
  right: -5px;
  padding: 16px;
  background-color: orange;
  border-radius: 50%;
}
</style>
