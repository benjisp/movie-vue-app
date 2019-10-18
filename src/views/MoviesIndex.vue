<template>
  <div class="movies-index">
    <h1>All Movies</h1>
    Search by Title:
    <input v-model="titleFilter" list="titles" />
    <datalist id="titles">
      <option v-for="movie in movies">{{ movie.title }}</option>
    </datalist>
    <div v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), 'year')">
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
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      movies: [],
      titleFilter: ""
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
