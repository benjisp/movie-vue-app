<template>
  <body>
    <div class="about">
      <h2>New Movie</h2>
      <div>
        Title:
        <input type="text" v-model="newMovieTitle" />
        Year:
        <input type="text" v-model="newMovieYear" />
        Plot:
        <input type="text" v-model="newMoviePlot" />
        Director:
        <input type="text" v-model="newMovieDirector" />
        <button v-on:click="createMovie()">Create Movie</button>
      </div>
      <h1>All Movies</h1>
      <div v-for="movie in movies">
        <h2>Title: {{ movie.title }}</h2>
        <button v-on:click="showMovie(movie)">More Info</button>
        <div v-if="currentMovie === movie">
          <h4>Year: {{ movie.year }}</h4>
          <h4>Plot: {{ movie.plot }}</h4>
          <h4>Director {{ movie.director }}</h4>
          <div>
            Title:
            <input type="text" v-model="movie.title" />
            Year:
            <input type="text" v-model="movie.year" />
            Plot:
            <input type="text" v-model="movie.plot" />
            Director:
            <input type="text" v-model="movie.director" />
            <button v-on:click="updateMovie(movie)">Update Movie</button>
            <button v-on:click="destroyMovie(movie)">Destroy Movie</button>
          </div>
        </div>
      </div>
    </div>
  </body>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      movies: [],
      currentMovie: {},
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      newMovieDirector: ""
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      this.movies = response.data;
    });
  },
  methods: {
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
        director: this.newMovieDirector
      };
      axios.post("/api/movies", params).then(response => {
        this.movies.push(response.data);
        this.newMovieTitle = "";
        this.newMovieYear = "";
        this.newMoviePlot = "";
        this.newMovieDirector = "";
      });
    },
    showMovie: function(movie) {
      if (this.currentMovie === movie) {
        this.currentMovie = {};
      } else {
        this.currentMovie = movie;
      }
    },
    updateMovie: function(movie) {
      var params = {
        title: movie.title,
        year: movie.year,
        plot: movie.plot,
        director: movie.director
      };
      axios.patch("/api/movies/" + movie.id, params).then(response => {
        this.currentMovie = {};
      });
    },
    destroyMovie: function(movie) {
      axios.delete("/api/movies/" + movie.id).then(response => {
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    }
  }
};
</script>
