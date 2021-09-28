<template>
  <div id="app">
    <Nav
    />
    <VideoList
    :videosArray="queryResults"
    :tvArray="queryResultsTv"
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
      queryDigested: '',
      queryResults: [],
      queryResultsTv: [],
      APIUrl : 'https://api.themoviedb.org/3/search/',
        typeOfVideos: {
            film:'movie',
            tvSeries: 'tv'
        },
        APILanguage:'&language=it_IT',
        APIKey : '?api_key=45ceb3fa9940738343c6705b54fdb57f',
        query: '&query='
        }
  },
  components: {
    Nav,
    VideoList,
  },
  created:{
    APICall();
  },
  methods: {
    queryStorer(text) {
      this.queryDigested = text
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
            }   
  }
}
</script>

<style lang="scss">
@import './assets/style/general.scss';

#app {
  height:100vh;
  width: 100vw;
  background-color: #090909;
  overflow-y: scroll;
}
</style>
