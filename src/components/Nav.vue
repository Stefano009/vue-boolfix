<template>
  <nav>
    <div class="title">
      <span>BOOLFIX</span>
    </div>
    <div class="search">
      <select name="genres" id="">
        <option v-for="genre in movieId" :value="genre.name" :key="genre.id"> {{ genre.name }}</option>
      </select>
      <input @keyup.enter="userQuery" v-model="searchMovie" type="text">
      <button @click="userQuery" >Search</button>
    </div>
  </nav>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Nav',
  data() {
    return{
      searchMovie: '',
      movieId: [],

    }
  },
  created: function() {
    this.APICallMovieGenre() 
    // this.APICallTv(this.firstResult) 
    },
  methods: {
    userQuery() {
      return this.$parent.APICall(this.searchMovie), this.$parent.APICallTv(this.searchMovie);
    },
     APICallMovieGenre() {
                axios.get('https://api.themoviedb.org/3/genre/movie/list?api_key=e99307154c6dfb0b4750f6603256716d')
                    .then(res => {
                        return this.movieId = res.data.genres
                    })
                    .catch(err => {
                        console.log(err)
                        })
        }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
nav {
  height: 40px;
  background-color: rgba(30, 30, 30, 1);
  display:flex;
  padding: 0 20px;
  justify-content: space-between;
  align-items: center;
  .title {
    color: red;
    height:100%;
    display: flex;
    align-items:center;
    padding-top: 10px; 
  }
}
</style>