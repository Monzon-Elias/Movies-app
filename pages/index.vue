<template>
  <div>
    <header>
      <h1>Movies Library App</h1>
      <input
        type="text"
        name="search"
        id="search"
        placeholder="type your movie here"
        v-model.lazy="searchInput"
        @keyup.enter="$fetch"
      />
      <span id="totalResults"></span>
      <button id="send" @click="searchMovies">Search!</button>
      <h4 id="noMovies"></h4>
    </header>
    <main id="listMovies">
      <div class="card" v-for="(movie, index) in searchedMovies" :key="index">
        <img
          :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
          alt="movie poster"
        />
        <div class="text">
          <p><b>Title: </b>{{ movie.title }}</p>
          <p>
            <b>Popularity Summary: </b>{{ movie.popularity }} out of
            {{ movie.vote_count }} votes
          </p>
        </div>
      </div>
    </main>
  </div>
</template>

<style>
body {
  background-color: beige;
  margin: 0;
}
header {
  font-family: "Gochi Hand", cursive;
  font-size: large;
  text-align: center;
  color: orange;
  position: fixed;
  top: 0;
  width: 100%;
  background-color: rgb(36, 107, 179);
  opacity: 0.95;
  border-bottom: 2px solid black;
}
header h1 {
  margin: 0;
}

button,
input {
  font-family: "Gochi Hand", cursive;
  font-size: large;
  padding: 0.2rem 0.5rem;
  border-radius: 0.5rem;
}

button:hover {
  background-color: rgb(76, 185, 76);
}
button:active {
  background-color: #3e8e41;
}

#listMovies {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  margin-top: 7rem;
}
.card {
  margin: 0.3rem;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  padding: 0.5rem;
  margin-bottom: 0.5rem;
}
.card img {
  width: 100%;
}
.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}
</style>
<script>
import axios from "axios";
export default {
  name: "home",
  head() {
    return {
      title: "Movie Library App",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Get all the movies",
        },
        {
          hid: "keywords",
          name: "keywords",
          content: "movies",
        },
      ],
    };
  },
  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: "",
    };
  },
  async fetch() {
    if (this.searchInput === "") {
      await this.getMovies();
      return;
    }
    await this.searchMovies();
  },

  methods: {
    async getMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/now_playing?api_key=25b1b277fd1952604770c24af13f6a18&language=en-US&include_adult=false&page=1`
      );
      const result = await data;
      result.data.results.forEach((movie) => {
        this.movies.push(movie);
      });
    },
    async searchMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=25b1b277fd1952604770c24af13f6a18&language=en-US&page=1&include_adult=false&query=${this.searchInput}`
      );
      const result = await data;
      result.data.results.forEach((movie) => {
        this.searchedMovies.push(movie);
      });
    },
  },
  watch: {
    searchInput() {
      console.log(this.searchInput);
    },
  },
};
</script>
