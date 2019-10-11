<template>
  <div class="movies-new">
    <h1>New Movie</h1>
    <form v-on:submit.prevent="createMovie()">
      <ul>
        <li v-for="error in errors">{{ error }}</li>
      </ul>
      Title:
      <input type="text" v-model="title" />
      Year:
      <input type="text" v-model="year" />
      Director:
      <input type="text" v-model="director" />
      Plot:
      <input type="text" v-model="plot" />
      <input type="submit" value="Create" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      title: "",
      year: "",
      director: "",
      plot: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
    createMovie: function() {
      var params = {
        title: this.title,
        year: this.year,
        director: this.director,
        plot: this.plot
      };
      axios
        .post("/api/movies", params)
        .then(response => {
          this.$router.push("/movies");
        })
        .catch(error => {
          console.log(error.response);
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
