<script setup>
import MovieCard from '../components/MovieCard.vue'
import { ref, onMounted, reactive, watch } from 'vue'
import axios from 'axios'

const movies = ref([])
const search = ref('')
const pageTitle = ref('Most popular movies')

async function getMoviesList() {
  const moviesResponse = await axios
    .get(
      'https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=en-US&page=1&sort_by=popularity.desc',
      {
        headers: {
          Authorization: `Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJmMGQ1MDQzNDY2NzQzOWJlYTExMGUwNDMyNzNlNWE2MCIsInN1YiI6IjY2MGU0ZTk5OTVjZTI0MDE3ZDZmZTU5MyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.qQrXiXFcqg9HbP7WbBs7pRmWjES4wG5H1ztfPgtedLQ`
        }
      }
    )
    .then((response) => {
      movies.value = response.data.results
    })
}
onMounted(async () => {
  await getMoviesList()
})

watch(search, (newSearch) => {
  if (newSearch && newSearch.length > 3) {
    movies.value = []
    pageTitle.value = 'Your search result'
    getSearchedMoviesList(newSearch)
  } else {
    pageTitle.value = 'Most popular movies'
    getMoviesList()
  }
})

async function getSearchedMoviesList(query) {
  const params = {
    query,
    page: 1,
    language: 'en-US',
    include_adult: 'false'
  }
  const moviesResponse = await axios
    .get(`https://api.themoviedb.org/3/search/movie`, {
      params,
      headers: {
        Authorization: `Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJmMGQ1MDQzNDY2NzQzOWJlYTExMGUwNDMyNzNlNWE2MCIsInN1YiI6IjY2MGU0ZTk5OTVjZTI0MDE3ZDZmZTU5MyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.qQrXiXFcqg9HbP7WbBs7pRmWjES4wG5H1ztfPgtedLQ`
      }
    })
    .then((response) => {
      movies.value = response.data.results
    })
}
</script>

<template>
  <main>
    <div class="mb-4">
      <input
        v-model="search"
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
        id="search"
        type="text"
        placeholder="Search"
      />
    </div>
    <div class="text-white text-xl text-center">{{ pageTitle }}</div>
    <div class="grid grid-cols-5 align-center mt-6 gap-10">
      <MovieCard v-for="movie in movies" :detail="movie" />
    </div>
  </main>
</template>
