<script setup>
import MovieCard from '../components/MovieCard.vue'
import { ref, onMounted, reactive } from 'vue'
import axios from 'axios'
import { useRoute } from 'vue-router'

const movie = ref([])
const route = useRoute()

async function getMovie() {
  const movieResponse = await axios
    .get(`https://api.themoviedb.org/3/movie/${route.params.movie}?language=en-US`, {
      headers: {
        Authorization: `Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJmMGQ1MDQzNDY2NzQzOWJlYTExMGUwNDMyNzNlNWE2MCIsInN1YiI6IjY2MGU0ZTk5OTVjZTI0MDE3ZDZmZTU5MyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.qQrXiXFcqg9HbP7WbBs7pRmWjES4wG5H1ztfPgtedLQ`
      }
    })
    .then((response) => {
      movie.value = response.data
    })
}

const openLink = (link) => {
  window.open(link)
}

onMounted(async () => {
  await getMovie()
})
</script>

<template>
  <div style="width: 100%">
    <div style="width: 600px; margin: auto">
      <div class="text-white text-xl">{{ movie.original_title }}</div>
      <div class="mt-3" style="width: 100%; height: 1px; background-color: white"></div>
      <div class="flex flex-row">
        <img
          :src="`https://image.tmdb.org/t/p/w300/${movie.poster_path}`"
          class="mt-6 border-4 border-gray-400"
        />
        <div class="flex flex-col mx-6">
          <div class="flex flex-row">
            <div class="mt-6 text-xl">{{ movie.release_date }}</div>
          </div>
          <div class="text-white mt-4" style="max-width: 300px">{{ movie.overview }}</div>
        </div>
      </div>
      <div class="flex flex-row">
        <button
          class="bg-transparent text-white mt-4 py-2 px-4 border border-white rounded"
          @click="$router.back()"
        >
          Go Back
        </button>
        <button
          class="bg-yellow-500 mt-4 mx-4 text-black font-bold py-2 px-4 rounded"
          @click="openLink(`https://www.imdb.com/title/${movie.imdb_id}/?ref_=fn_al_tt_1`)"
        >
          IMDB
        </button>
      </div>
    </div>
  </div>
</template>

<style></style>
