<template>
  <div class="movies-show">
    <h2>{{ movie.title }}</h2>
    <h3>{{ movie.year }}</h3>
    <h3>{{ movie.director }}</h3>
    <h3>{{ movie.plot }}</h3>
    <router-link v-bind:to="`/movies/${movie.id}/edit`"><button>Edit Movie</button></router-link>
    <br />
    <button v-on:click="destroyMovie(movie)">Delete Movie</button>
    <br />
    <router-link to="/movies">Back to All Movies</router-link>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      movie: {}
    };
  },
  created: function() {
    axios.get("/api/movies/" + this.$route.params.id).then(response => {
      this.movie = response.data;
    });
  },
  methods: {
    destroyMovie: function(movie) {
      axios.delete("/api/movies/" + movie.id).then(response => {
        this.$router.push("/movies");
      });
    }
  }
};
</script>
