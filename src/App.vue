<template>
  <div class="container">
    <div class="colum">
      <select v-model="selectedMovie" @change="selectMovie">
        <option disabled selected>Select One Movie</option>
        <option :key="movie.id" v-for="movie in movies" :value="movie.id">{{ movie.title }}</option>
      </select>
      <!-- <button class="movieButton" v-for="movie in movies" :key="movie.id" @click.prevent="selectMovie(movie.id)">{{ movie.title }}</button> -->
    </div>
    <div class="column">
      <template v-if="selectedMovieData">
        <p>Original Title: {{ selectedMovieData.original_title }}</p>
        <img class="poster" :src="selectedMovieData.movie_banner" :alt="selectedMovieData.description">
        <p>{{ selectedMovieData.description }}</p>
      </template>
      <p v-else>No Movie Selected</p>
    </div>
  </div>
</template>


<script setup>
import { ref, onBeforeMount } from 'vue'
import axios from 'axios'

const baseURL = 'https://ghibliapi.herokuapp.com/'

const instance = axios.create({
  baseURL,
})

const movies = ref(null)

const selectedMovie = ref(null)

onBeforeMount(async () => {
  const { data } = await instance.get('/films')
  movies.value = data;
})

const selectedMovieData = ref(null)

const selectMovie = async () => {
  const movie = selectedMovie.value
  const { data } = await instance.get(`/films/${ movie }`)
  selectedMovieData.value = data;
}
</script>

<style scoped>
.movieButton {
  display: block;
  width: 200px;
  margin-top: 20px;
  margin-right: 20px;
}

.container {
  display: flex;
}

.column {
  flex-direction: row;
}

.poster {
  max-width: 500px;
  max-height: 700px;
}
</style>