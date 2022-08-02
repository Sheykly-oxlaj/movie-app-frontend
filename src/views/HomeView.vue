<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
      newMovieParams: {},
      editMovieParams: {},
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("http://localhost:3000/movies.json").then((response) => {
        this.movies = response.data;
        console.log("All Movies: ", this.movies);
      });
    },
    createMovie: function () {
      axios
        .post("http://localhost:3000/movies.json", this.newMovieParams)
        .then((response) => {
          console.log("movie created ", response.data);
          this.movies.push(response.data);
          this.newmovieParams = {};
        })
        .catch((error) => (this.errorMessage = error));
    },
    showMovie: function (movie) {
      console.log(movie);
      document.querySelector("#movie-details").showModal();
      this.currentMovie = movie;
      this.editMovieParams = movie;
    },
    updateMovie: function (movie) {
      axios.patch("http://localhost:3000/movies/" + movie.id, this.editMovieParams).then((response) => {
        console.log("We done it!!!", response.data);
      });
    },
    destroyMovie: function (movie) {
      axios.delete("http://localhost:3000/movies/" + movie.id).then((response) => {
        console.log("Success!", response.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div>
      Title:
      <input type="text" v-model="newMovieParams.title" />
      Year:
      <input type="text" v-model="newMovieParams.year" />
      Plot:
      <input type="text" v-model="newMovieParams.plot" />
      Director:
      <input type="text" v-model="newMovieParams.director" />
    </div>
    <button v-on:click="createMovie()">Create Movie</button>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h3>{{ movie.name }}</h3>
      <h3>{{ movie.year }}</h3>
      <h3>{{ movie.plot }}</h3>
      <h3>{{ movie.director }}</h3>
      <p><button v-on:click="showMovie(movie)">More Information</button></p>
      <p>_______________________________________________________________________________</p>
    </div>
    <dialog id="movie-details">
      <form method="dialog">
        <h2>Movie Info</h2>
        <p>
          Title:
          <input type="text" v-model="editMovieParams.title" />
        </p>
        <p>
          Year:
          <input type="text" v-model="editMovieParams.year" />
        </p>
        <p>
          Plot:
          <input type="text" v-model="editMovieParams.plot" />
        </p>
        <p>
          Director:
          <input type="text" v-model="editMovieParams.director" />
        </p>
        <button v-on:click="updateMovie(currentMovie)">Update Movie</button>
        <button v-on:click="destroyMovie(currentMovie)">Delete Movie</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
