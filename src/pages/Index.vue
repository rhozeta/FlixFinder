<template>
  <q-page>

    <q-card class="my-card bg-secondary text-white">
      <div class="row">
        <div class='col col-md-8'>

      <q-card-section>
        <div class="text-h6">{{movieData[currentMovie].original_title}} - {{movieData[0].release_date.substring(0,4)}}</div>
        <div class="text-subtitle2">Original Language: {{movieData[currentMovie].original_language}}</div>
        <div class="text-subtitle2">Score: {{movieData[currentMovie].vote_average}} Votes: {{ movieData[currentMovie].vote_count}}</div>
        <div class="text-subtitle2">id: {{movieData[currentMovie].id}}</div>
        <div>Popularity: {{movieData[currentMovie].popularity}}</div>

      </q-card-section>

       <q-card-section>
        {{movieData[currentMovie].overview}}

      </q-card-section>
      <q-card-section>
         <!-- <q-video
          :ratio="16/9"
          src='https://www.youtube.com/embed/sfM7_JLk-84'
         /> -->
         <p>{{movieTrailerUrl}}</p>
      </q-card-section>
        </div>

      <q-card-section>

        <q-img :src='imagePath + movieData[currentMovie].poster_path' style='height: 770px;width: 500px;'></q-img>
      </q-card-section>
      </div>

      <q-separator dark />

      <q-card-actions>
        <q-btn flat @click="nextMovie">Yes</q-btn>
        <q-btn flat>No</q-btn>
      </q-card-actions>
    </q-card>

  </q-page>
</template>

<script>
import axios from 'axios'
export default {
  name: 'PageIndex',
  data () {
    return {
      movieData: {},
      movieGenres: {},
      pageNum: 1,
      movieVideoData: {},
      currentMovie: 0,
      imagePath: 'https://image.tmdb.org/t/p/original'
    }
  },
  methods: {
    fetchMovies: function () {
      axios
        .get('https://api.themoviedb.org/3/discover/movie/?api_key=094484b406f26d9e980758426e010055&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=' + this.pageNum)
        .then(response => (this.movieData = response.data.results))
      axios
        .get('http://api.themoviedb.org/3/movie/' + this.movieData[this.currentMovie].id + '/videos?api_key=094484b406f26d9e980758426e010055')
        .then(response => (this.movieVideoData = response.data.results))
    },
    nextMovie: function () {
      if (this.currentMovie < 19) {
        this.currentMovie++
        console.log(this.movieData[this.currentMovie])
        console.log(this.movieData[this.currentMovie].id)
        console.log(this.movieVideoData)
        console.log('http://api.themoviedb.org/3/movie/' + this.movieData[this.currentMovie].id + '/videos?api_key=094484b406f26d9e980758426e010055')
      } else {
        this.pageNum++
        this.CurrentMovie = 0
        this.fetchMovies()
      }
    }
  },
  mounted () {
    // axios
    //   .get('https://api.themoviedb.org/3/movie/3?api_key=094484b406f26d9e980758426e010055&language=en-US')
    //   .then(response => (this.movieData = response.data))

    this.fetchMovies()

    axios
      .get('https://api.themoviedb.org/3/genre/movie/list?api_key=094484b406f26d9e980758426e010055&language=en-US')
      .then(response => (this.movieGenres = response.data))
  }
}
</script>
