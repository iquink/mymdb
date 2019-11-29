<template>
<div>
  <div v-for="(genre, index) in genres.genres" > {{genre.id}} - {{genre.name}} </div>
  <div v-for="(movie, index) in movies.results"> {{movie.title}} - {{movie.original_title}}</div>

  </div>
</template>

<style lang="scss">

</style>

<script>
import Axios from 'axios'

export default {
  data() {
    return{
      genres: '',
      movies: ''
    }
  },
  created() {
    Axios.get('https://api.themoviedb.org/3/genre/movie/list?api_key=8e409bf9750ad60d1f84b3c57bb5bcdd&language=ru-RU')
    .then(response => {
  this.genres = response.data
  });
  Axios.get('https://api.themoviedb.org/3/movie/popular?api_key=8e409bf9750ad60d1f84b3c57bb5bcdd&language=ru-RU&page=1&region=ru')
    .then(response => {
  this.movies = response.data
  })
  
}
}
</script>