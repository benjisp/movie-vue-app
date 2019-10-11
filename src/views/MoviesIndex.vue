<template>
  <div class="movies-index">
    <h1>All Movies</h1>
    <div v-for="movie in movies">
      <h2>{{ movie.title }}</h2>
      <h3>{{ movie.year }}</h3>
      <h3>{{ movie.director }}</h3>
      <h3>{{ movie.plot }}</h3>
      <router-link v-bind:to="`/movies/${movie.id}`">More options</router-link>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      movies: []
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      this.movies = response.data;
    });
  },
  methods: {}
};
</script>
