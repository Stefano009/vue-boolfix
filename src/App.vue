<template>
  <div id="app">
    <Nav
    @option="selectedOption"
    />
    <VideoList
    :videosArray="queryResults"
    :tvArray="queryResultsTv"
    :option="option"
     />
  </div>
</template>

<script>
import Nav from './components/Nav.vue'
import VideoList from './components/VideoList.vue'
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      flag: false,
      option:'',
      queryDigested: '',
      queryResults: [],
      queryResultsTv: [],
      firstResult:'a',
      APIUrl : 'https://api.themoviedb.org/3/search/',
      typeOfVideos: {
          film:'movie',
          tvSeries: 'tv'
      },
      APILanguage:'&language=it_IT',
      APIKey : '?api_key=45ceb3fa9940738343c6705b54fdb57f',
      query: '&query=', 
      movieCredits: '/movie/{movie_id}/credits'
      }
  },
  components: {
    Nav,
    VideoList,
  },
  created: function() {
    this.APICall(this.firstResult) 
    this.APICallTv(this.firstResult) 
  },
  methods: {
    queryStorer(text) {
      this.queryDigested = text
    },
    selectedOption(number){
      this.option = number;
    },
    APICall(queryStorer) {
                axios.get(this.APIUrl+this.typeOfVideos.film+this.APIKey+this.APILanguage+this.query+queryStorer)
                    .then(res => {
                    this.queryResults = res.data.results
                    })
                    .catch(err => {
                        console.log(err)
                    })
            },
    APICallTv(queryStorer) {
                axios.get(this.APIUrl+this.typeOfVideos.tvSeries+this.APIKey+this.APILanguage+this.query+queryStorer)
                    .then(res => {
                    this.queryResultsTv = res.data.results
                    })
                    .catch(err => {
                        console.log(err)
                    })
          },
  }
}
</script>

<style lang="scss">
@import './assets/style/general.scss';
@import '~@fortawesome/fontawesome-free/css/all.min.css';

#app {
  height:100vh;
  width: 100vw;
  background-color: #090909;
  overflow-y: hidden;
}
</style>
